# `adoption/` — Product Adoption Benchmark feed

Per-client product adoption snapshots vs vertical benchmark, sourced from Meta CRM BoB Insights.

Feeds the **"Product Adoption Benchmark"** tab of each client cockpit in Hatch.

## Schema v1

```
adoption/<CLIENT>.json
{
  "meta": {
    "schema_version": "v1",
    "client": "<CLIENT>",
    "vertical_benchmark": "<industry segment, e.g. 'Banking and Credit Cards'>",
    "source": "Meta CRM BoB Insights",
    "snapshot_date": "YYYY-MM-DD",
    "period_label": "<e.g. 'Jan → Apr 2026'>",
    "geo_scope": ["BE","FR",...],
    "ad_accounts_included": ["Qonto FR","Qonto DE",...],
    "aggregation_method": "simple_average_across_accounts",
    "default_view": "ALL",
    "ui_hint": "sub_tabs"
  },
  "views": [
    {
      "id": "ALL" | "<AD_ACCOUNT_KEY>",
      "label": "<display label>",
      "ad_account_id": "<numeric ID>" | null,
      "categories": [
        {
          "key": "ai_solutions" | "creative_solutions" | "business_messaging_solutions" | "performance_boosters",
          "name": "<display name>",
          "client_pct_aggregate": <float>,
          "benchmark_pct_aggregate": <float>,
          "delta_pp_aggregate": <float>,
          "products": [
            {
              "name": "<product label>",
              "client_pct": <float|null>,
              "benchmark_pct": <float|null>,
              "delta_pp": <float|null>,
              "status": "scaling" | "active" | "greenfield"   // BM products only
            }
          ]
        }
      ]
    }
  ]
}
```

## Category mapping (4 categories, locked 2026-05-22)

| Category | Products |
|---|---|
| **AI Solutions** | Advantage+ sales / app / leads campaigns, A+ audience, A+ placements, A+ campaign budget |
| **Creative Solutions** | Reels opt-in, Partnership ads, A+ creative (full opt-in / add music / flexible media), Ad sets with 9:16 video, Ad sets with 5+ creatives, Catalog product video |
| **Business Messaging Solutions** | Click to WhatsApp, Click to Instagram Direct |
| **Performance Boosters** | 6+ Placements opt-in, Value rules, Lead ads (instant forms) |

## Status badge rule (Business Messaging only)

Applied to `client_pct` per product:
- `< 10%` → **greenfield**
- `10% – 40%` → **active**
- `> 40%` → **scaling**

## Aggregation rule

`ALL` view uses simple arithmetic mean across ad accounts, ignoring `--` (null) values. Spend-weighted averaging is not implemented because spend is not in the source CSV — if a `Spend` column is added, switch to weighted mean.

## Ownership contract — Hatch ↔ GitHub

| Field | Owner | Update cadence |
|---|---|---|
| `meta.*` | **GitHub** (CSV ingest) | When source CSV refreshes |
| `views[].categories[].products[].client_pct` / `benchmark_pct` / `delta_pp` | **GitHub** (raw from CSV) | Each CSV refresh |
| `views[].categories[].*_aggregate` | **GitHub** (computed from CSV) | Each CSV refresh |
| `views[].categories[].products[].status` (BM) | **GitHub** (rule-based) | Each CSV refresh |
| Category keys / names / ordering | **GitHub** (configuration) | Stable, rarely changes |
| Executive summary header (1-line narrative) | **Hatch** (LLM-generated from data) | On each sync |
| Footer callout per category | **Hatch** (LLM-generated from deltas) | On each sync |
| Layout / colors / progress bars / tab UI | **Hatch** | N/A |

## UI rendering hint

`meta.ui_hint = "sub_tabs"` → Hatch should render `views[]` as horizontal sub-tabs within the Product Adoption Benchmark tab, with `views[0]` (the `ALL` view) as the default selected tab.

## Pipeline

1. Julian exports CSV from Meta CRM BoB Insights → Drive
2. Pipeline parses CSV → builds `adoption/<CLIENT>.json` per the schema above
3. Pushes JSON to `j-viala/client-roadmaps:main`
4. Hatch syncs from GitHub raw URL on its 2h cadence
5. Hatch renders sub-tabs + auto-generates executive summary / footer callouts via LLM
