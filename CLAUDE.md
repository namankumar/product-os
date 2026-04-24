# {{YOUR_NAME}} — Claude Context

## Current Role
{{YOUR_ROLE}}. Building {{PRODUCT}}.

## Active Projects
- **{{PRODUCT}}**: {{ONE_LINE_DESCRIPTION}}
  - Launch: {{LAUNCH_DATE}}
  - Status: {{STATUS}}

## Team

| Name | Role |
|---|---|
| {{FOUNDER_CEO}} | Founder, CEO |
| {{COO}} | Founder, COO. Your manager |
| {{ENG_LEAD}} | Engineering lead |
| {{PROJECT_MANAGER}} | Project manager |
| {{DIRECT_REPORT}} | Your direct report |

## Product Glossary

Internal definitions for Claude's accuracy when writing. Don't explain these in docs — assume your audience knows the domain.

| Term | Definition |
|---|---|
| {{TERM_1}} | {{DEFINITION_1}} |
| {{TERM_2}} | {{DEFINITION_2}} |
| {{TERM_3}} | {{DEFINITION_3}} |

## Skills

All skills live in `.claude/skills/`. Invoked with `/name` or by referencing the skill name. Each skill file contains the full process, rules, and output format.

**Context loading is mandatory but on-demand.** Every skill declares a `## Reads` section. Load those files when the skill needs them, not all upfront. The `## Reads` list tells you what's available, not what to preload. Read the files that are relevant to the specific question or step you're working on. This is what makes answers relevant instead of generic. Skip this and the output will miss what we already know.

| Skill | What it does |
|---|---|
| competitive-analysis | Deep dive on competitors/protocols, extract learnings |
| strategy | Create executive-grade strategy docs |
| narrative | Create positioning/narrative docs |
| feedback | Collect and prioritize feedback |
| tasks | Organize todos and surface urgent work |
| roadmap | Manage product roadmap (features, not tasks) |
| comms | Create leadership/investor updates |
| shape | Give form to a fuzzy idea before committing to a PRD or strategy doc |
| prd | Create PRDs, briefs, checklists, decision logs |
| market-research | Scan landscape, market trends, inform roadmap |
| ux | UX audits, competitive benchmarks, funnel analysis, craft proposals |
| status | Track workstream status across projects, dependencies, blockers, escalations |
| meeting-notes | Scan email for meeting notes, fetch transcripts, extract decisions and commitments |
| brief | Daily or weekly briefing. Daily: morning kill list, signals. Weekly: 2-week view, trends, next week plan |
| users | Track potential users, manage relationships, surface follow-ups |
| reply | Craft replies in your voice for any channel |
| xgrow | Daily X/Twitter content engine: find posts to engage with, draft replies and originals |
| xarticle | Long-form X articles. Research-backed, evidence-dense, designed for dwell time |
| lgrow | Weekly LinkedIn content engine: draft posts and comments |
| harvest | Extract experience bank entries from system-building conversations |
| essay | Write expressive, well-reasoned, persuasive essays |

### Library skills (used by other skills, not invoked directly)

| Skill | File | What it does |
|---|---|---|
| voice | `.claude/skills/utils/voice.md` | Your voice + anti-LLM writing rules |
| writeDocs | `.claude/skills/utils/writeDocs.md` | Edit/rewrite existing docs: narrative rules, framing, propagate fixes |
| google-docs-formatting | `.claude/skills/utils/google-docs-formatting.md` | Heading sizes, parsing technique, push-to-doc process |

## Data Sources

Define your external data sources in `.claude/skills/utils/sources.md`. That file is the single source of truth for: which MCPs to call, how to scan them, cache file locations, freshness thresholds, and cache templates.

**Selective access:** When you ask to check a specific source (e.g., "check Intercom", "what's in Linear"), Claude reads `sources.md` for that source's scanning instructions. Don't run the full status skill unless a full scan is requested.

**New sources:** Whenever a new MCP or external source is added, add it to `sources.md` first. No source exists until it's in `sources.md`.

## Working with Historical Context

**Before answering questions or starting new research, ALWAYS:**
1. Search `docs/` for existing analysis
2. Read relevant files to understand what we already know
3. Build on existing research rather than duplicating work
4. Only invoke new research agents if there are gaps or explicit request for fresh analysis

**Look in the right folder by topic:**
- Context (current context, OKRs, narratives, artifacts, cadences): `context/`
- Strategy: `docs/strategy/`
- Narrative/positioning: `docs/narrative/`
- GTM: `docs/gtm/`
- Market research & competitive: `docs/market-research/`
- Product (PRDs, specs): `docs/product/`
- Comms (updates, digests): `cache/comms/`
- Feedback & user tracking: `docs/users/`

**The docs folder is institutional memory.** Treat existing docs as the foundation, not optional context.

## Open Todos
See `tasks-db.md`

## Writing Rules (Strategy Docs, Execution Plans, PRDs)
- **Always read `.claude/skills/utils/writeDocs.md` before writing or editing any doc.**
- **All writing has a narrative.** Know the throughline before writing. Factual docs carry narrative through structure. Persuasive docs carry it overtly in prose.
- **Voice:** See `.claude/skills/utils/voice.md`. All anti-LLM rules, redundancy checks, and rhythm rules live there.
- **No bracket explanations** that restate what was just said. Pick one framing, commit to it.
- **No em dashes.** Use periods or commas instead.
- **Tables for comparisons, prose for arguments.**
- **State things once.** Don't recap, don't add summary sentences, don't restate the bold header in the first sentence.
- **When feedback reveals a wrong assumption, fix ALL instances** across the doc, not just the line flagged.

## Context Extraction (always on)
- **Every interaction with any data source is an extraction opportunity.** While doing any task, always notice context with lasting value beyond the current task.
- **Two input streams:** conversation (real-time) and notes (async/mobile). Both are always-on.
- **Extract, confirm, file.** Surface what was found, where it should go, why it matters. Confirm before writing to a persistent file.
- **What to extract:** decision context (`context/current-context.md`), narrative shifts (`context/narrative-map.md`), how people operate (`context/room/`), hard metrics (`context/okr-status.md`), feature ideas (`docs/product/`), commitments (`tasks-db.md`).

## Google Docs Formatting
- **ALWAYS format headings when pushing to Google Docs.** See `.claude/skills/utils/google-docs-formatting.md`.
- **Reset first, then apply.** Clear all formatting to default first, then selectively apply bold/size.
- **Voice skill applies to docs too.**

## Harvest Reminder
- **At the end of any session with meaningful back-and-forth**, prompt: "Want to run `/harvest` before we close?" Don't ask after simple lookups. Ask when the conversation itself contains signal worth capturing.

## Claude Rules
- **Google Docs:** Always write to the same Google Doc for a given file. Don't create new docs. Store the link at the top or bottom of the local `.md` file.
- **Local first:** All files saved locally as `.md` in the appropriate `docs/<category>/` subfolder.
- **Writing skill is mandatory.** Read `.claude/skills/utils/writeDocs.md` before writing or editing any doc.
- **Every item needs a source.** Every action item, status update, signal, or finding must include where it came from.

## Directory Structure

- **`context/`** = current state of the world (OKRs, narratives, org dynamics, cadences)
- **`docs/`** = what you produce (strategy, PRDs, market research, feedback, user tracking)
- **`cache/`** = machine-generated scan data (MCP caches, not your thinking)
- **`projects/`** = personal projects, separate from work

| Folder | Purpose |
|---|---|
| `context/` | Current state: OKRs, narratives, org dynamics, artifacts, cadences |
| `cache/comms/` | Leadership updates, investor comms, digests |
| `context/room/` | Group dynamics analysis (one file per group, persistent) |
| `docs/strategy/` | Strategy docs, execution plans, positioning |
| `docs/narrative/` | Messaging, narrative docs |
| `docs/gtm/` | Go-to-market plans, launch docs |
| `docs/market-research/` | Competitor analyses, market sizing, landscape scans |
| `docs/product/` | PRDs, specs, roadmap |
| `docs/users/` | User tracking, relationships, follow-ups, processed feedback |
| `cache/` | MCP scan caches |
| `projects/growsocial/` | Social growth engine |
| `.claude/skills/` | Skill definitions (Claude Code skills, invoked with /name) |
| `tasks-db.md` | Open todos |
| `done.md` | Completed items |

---
*Fork this repo and replace all `{{PLACEHOLDERS}}` with your own context.*
