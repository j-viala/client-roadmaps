# client-roadmaps

Public, sanitized partnership roadmaps per client.
Source of truth consumed by **Hatch** (per-client partnership cockpits) and by internal Tech Pod tooling.

## Files

- `_schema.json` — canonical schema (Hatch-compatible v1.0)
- `clients/<CLIENT>.json` — per-client roadmap (one file per client)

## Schema (per initiative)

| Field | Type | Notes |
|---|---|---|
| `id` | integer | Unique within client (1, 2, 3, ...) |
| `initiative` | string | Short title (client-facing) |
| `business_impact` | string | One-line value statement |
| `current_situation` | string | Status narrative |
| `next_action` | string | What's next |
| `meta_owner` | string | Meta-side owner |
| `client_owner` | string | Client-side owner |
| `theme` | enum | `Performance & Growth` / `Measurement & Signals` / `Creative & Storytelling` / `AI & Innovation` / `Strategic Expansion` |
| `status` | enum | `Open` / `In Progress` / `Blocked` / `Done` |
| `impact_tier` | enum | `High` / `Medium` / `Future` |
| `deadline` | ISO date or `""` | Optional |
| `last_updated` | ISO date | YYYY-MM-DD |
| `link` | string | Optional reference URL |
| `tags` | string | Optional, comma-separated |
| `notes` | string | Optional public-safe context |

Top-level: `client`, `hub`, `schema_version`, `last_sync`, `initiatives[]`, optional `_display`.

## Hatch ingestion

```
https://raw.githubusercontent.com/j-viala/client-roadmaps/main/clients/<CLIENT>.json
```

No auth required (public repo). Recommended sync mode: cron 15-30 min.

## Update workflow

1. Edit `clients/<CLIENT>.json` directly via GitHub UI, commit, or push from a script.
2. Hatch fetches the raw URL on its cron and refreshes its dashboards.
3. **Never commit revenue figures, opp_size, sales/RevOps internal context, or any data not appropriate for client viewing.**
