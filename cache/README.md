# cache/

Machine-generated scan data. Not your thinking — MCP outputs, digests, and feed snapshots. Treated as ephemeral: always regeneratable, never the source of truth.

| Folder | What goes here |
|---|---|
| `comms/` | Leadership updates, investor comms, Slack digests |
| `slack/` | Slack channel scan outputs |
| `linear/` | Linear issue snapshots |
| `analytics/` | Mixpanel / Amplitude query results |

**Freshness:** cache files expire. Check the `updated:` timestamp at the top of each file before trusting the data. Re-run the relevant skill to refresh.
