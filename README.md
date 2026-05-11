# client-roadmaps

Public, sanitized partnership roadmaps per client.
Source of truth consumed by Hatch (per-client partnership cockpits) and by internal Tech Pod tooling.

## Files

- `_schema.json` — canonical schema definition for `clients/*.json`
- `clients/<CLIENT>.json` — per-client roadmap (one file per client)

## Schema (per initiative)

| Field | Type | Description |
|---|---|---|
| `id` | string | Stable unique ID (e.g. `MWM-001`) |
| `initiative` | string | Short title (client-facing) |
| `business_impact` | string | One-line value statement (client-facing) |
| `current_situation` | string | Status narrative (client-facing) |
| `next_action` | string | What's next (client-facing) |
| `meta_owner` | string | Meta-side owner (e.g. `Julian`, `Rafael`) |
| `client_owner` | string | Client-side owner (e.g. `Josselyn`) |
| `theme` | string | Section grouping (e.g. `Performance & Growth`, `Measurement & Insights`) |
| `status` | enum | `needs_action` / `in_progress` / `completed` / `blocked` |
| `impact_tier` | enum | `high` / `medium` / `low` |
| `last_updated` | ISO date | YYYY-MM-DD |
| `notes` | string | Optional public-safe context |

## Update workflow

1. Edit `clients/<CLIENT>.json` directly via GitHub UI or commit.
2. Hatch fetches the raw URL on a 15-30 min cron and refreshes its dashboards.
3. **Never commit revenue figures, Sales/RevOps internal context, opp_size, or any data not appropriate for client viewing.**

## Hatch ingestion

Public raw URL pattern:
```
https://raw.githubusercontent.com/j-viala/client-roadmaps/main/clients/<CLIENT>.json
```

No auth required.
