# client-roadmaps

Public, sanitized partnership roadmaps per client.
Source of truth consumed by **Hatch** (per-client partnership cockpits).

## Files

- `_schema.json` — canonical schema (Hatch v2)
- `clients/<CLIENT>.json` — per-client roadmap (one file per client, flat array of initiatives)

## Schema v2 (per initiative)

| Field | Type | Notes |
|---|---|---|
| `stream_name` | string | Title (client-facing) |
| `subtitle_why` | string | Why it matters — business rationale |
| `subtitle_how` | string | How we deliver — current state + next step |
| `theme` | string | Client-specific. Examples: MWM uses `App Performance` / `Measurement & Signals` / `Creative & Formats` / `AI / Innovation` / `Strategic Account`. SumUp uses `B2C Brand` / `B2B Merchant Acquisition` / `Measurement & Signals` / `WhatsApp Business` / `Geo Expansion`. Each client defines their own theme vocabulary. |
| `status` | enum | `Open` / `In Progress` / `Blocked` / `Done` |
| `meta_owner` | string | Meta-side owner |
| `client_owner` | string | Client-side owner (generic, replicable across hubs) |
| `deadline` | ISO date or `""` | Optional |
| `link_url` | string | Optional reference URL |
| `tags` | string | Optional, comma-separated. Convention: `WAITING:Name` for blockers |
| `sort_order` | integer | Display order |
| `pinned` | boolean | If true, renders pinned at top of theme |

**Top-level format:** flat JSON array (no wrapper object).

## Hatch ingestion

```
https://raw.githubusercontent.com/j-viala/client-roadmaps/main/clients/<CLIENT>.json
```

No auth required (public repo). Sync mode: cron 2h.

## Update workflow

1. Edit `clients/<CLIENT>.json` directly via GitHub UI, commit, or push from a script.
2. Hatch fetches the raw URL on its 2h cron and refreshes its dashboards.
3. **Never commit revenue figures, opp_size, sales/RevOps internal context, or any data not appropriate for client viewing.**
