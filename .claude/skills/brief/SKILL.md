---
name: brief
description: Daily or weekly briefing with cross-references and signals
---

# Brief

## Purpose

Daily or weekly briefing. Same process, different window. Daily is the default morning briefing. Weekly widens to 14 days, adds trend analysis, next-week planning. After the weekly brief is complete, run the comms skill to draft the Monday company update.

## Mode

**Daily** (default): `/daily`, morning briefing
- Window: last 24h
- Output: `cache/brief/brief-[YYYY-MM-DD].md`

**Weekly**: `/weekly`, end-of-week briefing
- Window: last 14 days
- Reads all daily briefs from the period
- Adds: What Shipped, Trends, OKRs, Stale, Next Week
- Output: `cache/brief/weekly-[YYYY-MM-DD].md`

## Reads

The brief does NOT read cache files in the main thread. It delegates all heavy reads to a single dedicated agent (Agent D) that gets a fresh 200K-token context window with zero conversation history. The main thread only orchestrates and delivers.

### Main thread reads
- `cache/brief/brief-[YYYY-MM-DD].md` — the final brief (written by Agent D), for display to {{YOUR_NAME}}

### Agent D reads

**Cache files:**
- `cache/slack.md` — Slack conversations, action items, signals
- `cache/signal.md` — Signal group messages, feedback, strategic signals
- `cache/linear.md` — Linear issues (mobile engineering)
- `cache/monday.md` — Monday items (extension + design)
- `cache/github.md` — GitHub issues (extension)
- `cache/gdocs.md` — Google Docs (meeting notes, decisions)
- `cache/feedback-monday.md` — Monday feedback board
- `cache/apple-notes.md` — Apple Notes ({{YOUR_NAME}}'s scratchpad)
- `cache/mixpanel.md` — Mixpanel product analytics
- `cache/intercom.md` — Intercom support conversations
- `cache/ga.md` — Google Analytics ({{PRODUCT}} website)
- `cache/ga-extension.md` — Google Analytics (Chrome extension listing)
- `cache/cws.md` — Chrome Web Store installs/users
- `cache/play-console.md` — Google Play Console installs/devices
- `cache/app-store.md` — App Store Connect (iOS, blocked)
- `cache/near-intents.md` — NEAR Intents swap data

**Context files:**
- `cache/brief/brief-*.md` — yesterday's daily (follow-through loop)
- `cache/tasks/todos-[YYYY-MM-DD].md` — today's Kill List (produced by tasks skill)
- `cache/status/status-*.md` — latest status snapshot
- `docs/product/shield-funnel.md` — unified funnel data, OKR pace, conversion chain
- `docs/product/roadmap-prospective.md` — workstream themes
- `context/current-context.md` — decision-making context
- `context/cadences.md` — what's due, what's converging
- `context/okr-status.md` — OKR targets vs actuals
- `context/narrative-map.md` — active narratives
- `context/org-dynamics.md` — political context
- `context/room/` — room analysis files
- `cache/feedback-monday.md` — feedback state, bug status (Monday board cache)
- `docs/users/users.md` — active user relationships (if exists)
- `cache/tasks/tasks-db.md` — for full state append and upcoming dates
- `cache/brief/brief-*.md` — [weekly] all daily briefs from last 14 days
- `docs/product/master-shield-prd-roadmap.md` — [weekly] roadmap for next-week planning

**Agent D reads all of these in a fresh context with zero conversation history.**

## Writes
- `cache/brief/brief-[YYYY-MM-DD].md` — the daily briefing
- `context/okr-status.md` — if metrics changed
- `context/narrative-map.md` — if narrative shifts detected
- `context/room/` — updated room files (via read-room)
- `cache/comms/slack-digest.md` — append Slack findings
- `cache/tasks/tasks-db.md` — new action items found during brief (tasks skill handles the full refresh)
- `cache/tasks/done.md` — newly completed items
- `cache/feedback-monday.md` — if new feedback found
- `cache/brief/weekly-[YYYY-MM-DD].md` — [weekly] the weekly brief

## Process

The brief runs in 2 phases, both orchestrated by the main thread:
1. **Refresh** — main thread launches parallel agents to scan stale sources. Each writes to its cache file and returns status + delta. Main thread collects the deltas.
2. **Brief** — main thread launches Agent D as a subagent. Agent D gets a fresh context, reads ALL caches + context files, cross-references thoroughly, writes the final brief.

The main thread never reads cache files. It orchestrates and delivers.

### Phase 1: Refresh Data

**Freshness check first (biggest time-saver).** Before launching ANY agents, check ALL cache file timestamps against freshness thresholds in `sources.md`. If ALL caches are fresh AND funnel + meeting-notes caches are not overdue, skip Phase 1 entirely and launch Agent D immediately. Do not launch funnel or meeting-notes refresh agents unless their caches are stale. On days with fresh caches this saves the entire Phase 1 window.

**HARD GATE: Do NOT launch Agent D until all 4-hour-threshold sources are fresh.** Slack, Linear, Monday, and Intercom all have 4-hour thresholds. If any of these are stale, refresh them BEFORE launching Agent D. No exceptions. A brief built on stale conversation data produces false signals — stale convergent analysis is worse than no analysis because it creates false urgency. Check freshness by comparing the cache file's `Last refreshed` timestamp against the current time. Never assume a cache is fresh because "we refreshed it earlier in the session" — that refresh may have been for a different skill or a different time window. Always use absolute timestamps.

If any cache is stale, refresh per the **Parallel Scan Protocol** in `sources.md`. Each refresh agent:
1. Reads the existing cache (baseline for delta)
2. Scans the source via MCP/agent-browser
3. Writes results directly to the cache file
4. Returns STATUS + DELTA (what changed vs previous cache, 3-5 bullets max)

Do NOT have refresh agents return full scraped data. The cache file is the record.

**Also run as parallel agents (only if their caches are stale):**
- **Funnel skill** (`skills/funnel.md`) — refresh store and GA data into `docs/product/shield-funnel.md`.
- **Meeting notes skill** (`skills/meeting-notes.md`) — scan email for new meeting transcripts, extract decisions, commitments, people signals into `context/meeting-notes.md`.

**After refresh agents complete — launch simultaneously:**

**Room refresh** (updates `context/room/`) and **tasks skill** run in parallel — they are independent:
- **Room refresh:** Check if new messages exist since each room file's `Last updated` date. Use cached Slack/Signal data, don't re-scan. If yes, run read-room to update the analysis.
- **Tasks skill** (`skills/tasks.md`): Refresh `cache/tasks/tasks-db.md` and produce `cache/tasks/todos-[YYYY-MM-DD].md`. Reads now-fresh caches, extracts {{YOUR_NAME}}-relevant items, dedupes, organizes into Kill List + Parking Lot.

Launch both simultaneously. Phase 2 (Agent D) launches only after both complete.

If any source is unavailable, note it and continue.

### Phase 2: Agent D — Read Everything, Cross-Reference, Write Brief

Launch a single agent (Agent D) with a fresh context window. This agent reads ALL 16 cache files + all context files, then cross-references thoroughly before writing.

Agent D reads all cache files and context files listed in the Reads section above. It has a fresh context with no conversation history, so it can hold everything.

#### What Agent D writes

`cache/brief/brief-[YYYY-MM-DD].md` — the complete final brief with all sections.

#### How Agent D cross-references

This is the core value. Agent D has ALL data in one context, so it can do what no split architecture can: connect any signal to any other signal across any source.

**Cross-referencing protocol (mandatory, not optional):**

After reading all files, Agent D must run these passes before writing any output:

**Pass 1 — Extract raw signals.** Scan every cache file and pull out every discrete signal: metric changes, status changes, new items, blockers, decisions, asks, commitments, feedback, competitive intel, people movements. Tag each with source + person + date.

**Pass 2 — Match across sources.** For every signal from Pass 1, check: does this appear in or connect to any other source?
- A Slack message about a bug → does Linear/Monday have a ticket for it? If not, it's untracked.
- A metric drop → does any Slack/Signal conversation explain it? Does a stale ticket correlate?
- A blocker in Linear → did someone mention it on Slack? Is anyone escalating?
- A commitment someone made → is there a ticket? Is there a follow-up?
- A feedback theme → does it map to a roadmap item? A "not doing" item? Neither?
- A competitive signal → does it strengthen or weaken an active narrative?
- A people signal → does it align with or contradict the room analysis?

**Pass 3 — Identify convergent signals.** Which signals from different sources point at the same underlying issue? These are the lead items. A metric drop + a stale ticket + a feedback theme = one story, not three bullets in three sections.

**Pass 4 — Follow-through.** Compare against yesterday's brief:
- Kill list items: done or carried over?
- Moves proposed: did they land?
- Blockers flagged: resolved or still stuck?
- Predictions made: confirmed or contradicted?

**Pass 5 — Strategic implications.** For the biggest convergent signals:
- What assumption does this challenge?
- What should {{YOUR_NAME}} do about it?
- Who needs to know?
- What's the cost of ignoring it for another day?

**Pass 6 — Write.** Now write the brief, with convergent signals driving the TL;DR and Kill List, not just sitting in their own section.

**Action item extraction (during Pass 1):**
Scan Slack, Signal, GDocs, Apple Notes for:
- Direct asks to {{YOUR_NAME}} ("can you...", "@naman", "waiting on you for...")
- Commitments {{YOUR_NAME}} made ("I'll...", "let me...", "will send...")
- Commitments others made TO {{YOUR_NAME}} ("I'll get you X by Y")
- Decisions deferred ("let's circle back", "need to think about", "TBD")
Each gets: what, who, source link, date, suggested deadline. Add to `cache/tasks/tasks-db.md` if not already tracked.

**Week-in-context:** Adapt the lens to where you are in the week. Monday: what are we aiming to ship? Tuesday-Thursday: are we on pace? Friday: did we hit it? What carries?

**Look ahead (next 2-3 days):**
- `context/cadences.md` — meetings, deadlines, recurring events
- `cache/tasks/tasks-db.md` — items with dates in the next few days

**[Weekly only]:** Also read all daily briefs from last 14 days. Extend all passes to 14-day window. Build the Next Week plan. Produce the weekly format instead of daily.

#### Agent D prompt template

> You are writing the daily brief for [DATE]. You have access to the full project context.
>
> **Step 0:** Before reading all files, check which optional files exist and are non-empty: `cache/app-store.md`, `cache/near-intents.md`, `cache/github.md`. Skip any that are empty or missing — do not spend time loading them. For any cache file not modified today, note it as potentially stale when referencing its data.
>
> **Step 1:** Read ALL of these files (skipping any confirmed empty/missing from Step 0):
> - All 16 cache files in `cache/`: slack.md, signal.md, linear.md, monday.md, github.md, gdocs.md, feedback-monday.md, apple-notes.md, mixpanel.md, intercom.md, ga.md, ga-extension.md, cws.md, play-console.md, app-store.md, near-intents.md
> - Yesterday's brief: most recent `cache/brief/brief-*.md`
> - Today's todos: `cache/tasks/todos-[YYYY-MM-DD].md`
> - Latest status: most recent `cache/status/status-*.md`
> - Funnel: `docs/product/shield-funnel.md`
> - Themes: `docs/product/roadmap-prospective.md`
> - Context: `context/current-context.md`
> - Cadences: `context/cadences.md`
> - OKRs: `context/okr-status.md`
> - Narratives: `context/narrative-map.md`
> - Org dynamics: `context/org-dynamics.md`
> - Room files: `context/room/` (all files)
> - Feedback: `cache/feedback-monday.md`
> - Users: `docs/users/users.md` (if exists)
> - Tasks: `cache/tasks/tasks-db.md`
>
> **Step 2:** Run the 6-pass cross-referencing protocol from the brief skill (extract signals → match across sources → identify convergent signals → follow-through vs yesterday → strategic implications → write).
>
> **Step 3:** Write the final brief to `cache/brief/brief-[YYYY-MM-DD].md` using the output format from `skills/brief.md`. Append full `cache/tasks/tasks-db.md` at bottom under `## Full Task State`. Update `cache/tasks/tasks-db.md` with any new action items extracted.
>
> **Step 4:** Return: "DONE: [1-line summary of the day's story]" plus any proposed context extraction diffs for leadership files (exact line, which file, which section, why).

**Speed summary:** If all caches are fresh, Phase 1 is skipped entirely — Agent D launches immediately. Room refresh + tasks skill run in parallel after Phase 1 (saves ~2-3 min vs sequential). Agent D skips empty/missing optional files (app-store, near-intents, github).

### Phase 3: Deliver (main thread)

After Agent D completes, the main thread:

1. Reads `cache/brief/brief-[YYYY-MM-DD].md` (already written by Agent D)
2. Displays it to {{YOUR_NAME}}
3. Presents any context extraction diffs Agent D proposed for leadership files. For each: "I'd add this line to [file] under [section]: '[exact text]'. Confirm?"
4. Applies confirmed leadership file updates:
   - **`context/okr-status.md`** — update Actual column if metrics changed
   - **`context/narrative-map.md`** — update if narrative strength shifted
   - **`context/room/`** — already handled in Phase 1
   - **Do NOT update `context/current-context.md`** — only from {{YOUR_NAME}}'s conversation
   - **Do NOT update `context/org-dynamics.md`** — flag signals, let {{YOUR_NAME}} decide
5. **[Weekly only]:** Invokes the comms skill (Monday Company Update type) to draft the company update

## Output Format

```markdown
# Daily — [Day, Month DD]

**Sources refreshed:** Linear ✓/✗, Monday ✓/✗, Slack ✓/✗, Signal ✓/✗, Mixpanel ✓/✗, Intercom ✓/✗, Apple Notes ✓/✗, Sheets ✓/✗, Room files ✓/✗

## TL;DR
[3 sentences max. Only include when 8+ sections have content. The one thing that matters most today, the biggest risk, and what changed overnight. Skip on quiet days.]

## Kill List
[4-6 items. Ranked by leverage, not urgency. Leverage = who's waiting × what it
unblocks downstream × whether delay compounds.]

1. **[Task]** — [who's waiting + their influence] — [what it unblocks] — [cost of another day's delay]

For each item, answer: why is THIS the highest-leverage thing today, not just the most urgent?

**Quick batch (~Xmin):**
- [micro-task]

**Drop or delegate:**
- [Item that's been on the kill list 3+ days] — [suggested delegation or cut]

## Workstreams

[One bullet per active theme. Themes come from the section headers in `docs/product/roadmap-prospective.md`. Read that file to get the current theme list. Only show themes with activity. Flag any theme that changed color since yesterday's daily and why.]

- **[Theme]** 🟢/🟡/🔴 [↑/↓ if changed] — [what's happening, what's blocked, what's next]

## What Moved (last 24h)
[What shipped, progressed, or got unblocked. Every item has a source link.]

- [Thing] — [status change] — [source + link]

**What didn't move (should have):**
- [Item] — in progress [X days], no Slack/Linear activity since [date] — [owner]

Pull stale items from the status snapshot. Anything in progress 5+ days with no visible activity belongs here. This is often a louder signal than what shipped.

## Decisions Made
[Decisions finalized in the last 24h. Skip if none.]

- [Decision] — [who decided] — [downstream work it triggers] — [source + link]

## Blocking / Blocked
| Direction | Who | What | Since | Days | Risk |
|-----------|-----|------|-------|------|------|
| I'm blocking | [person] | [task] | [date] | [n] | 🟡/🔴 |
| Blocked by | [person] | [task] | [date] | [n] | 🟡/🔴 |

Risk = 🔴 when: blocked 3+ days, OR the person has escalated, OR downstream items are piling up. 🟡 when: 1-2 days, no escalation yet but delay is compounding.

**Suggested unblocks:**
- [blocked item]: [specific action — "send X to Y", "make decision on Z", "delegate to {{DIRECT_REPORT}}"]

## Metrics (from Mixpanel + Funnel)

### Usage
| Metric | Value | Change (WoW) |
|---|---|---|
| DAU (combined) | | |
| WAU | | |

### Cumulative Users
| Metric | Extension | Mobile | Total |
|---|---|---|---|
| Onboarded (all-time) | | | |
| Transaction users (all-time) | | | |

### User Settings
| Setting | Extension | Mobile |
|---|---|---|
| Auto-join ON | | |
| Auto-join OFF | | |
| Local proving users | | |
| Delegated/remote proving users | | |

**Mixpanel events:**
- Auto-join: `wallet_auto_join_toggled` (ext) / `wallet_mobile_auto_join_toggled` (mobile), property `enabled`
- Proving: `wallet_proving_mode_changed` (ext) / `wallet_mobile_proving_mode_changed` (mobile). No mode property — use mobile proxy: `wallet_mobile_local_proving_initiated` vs `wallet_mobile_remote_proving_initiated`
- Cumulative: `wallet_onboarding_complete` / `wallet_mobile_onboarding_complete`, unique count
- Tx users: `wallet_transaction_completed` / `wallet_mobile_transaction_completed`, unique count

### OKR Pace (from `shield-funnel.md`)
| OKR | Target | Current | Pace status |
|---|---|---|---|
| Downloads (KR2) | 3,500 | | |
| Transfers (O3) | 5,000 | | |
| Funded accounts (KR3) | 1,000 | | |

### Conversion Chain (from `shield-funnel.md`)
[Reference the funnel doc's conversion chain. Highlight the biggest drop-off and any WoW regressions >20%.]

[Flag notable spikes, drops, or funnel breakpoints. Skip if no change.]

**What's the story:**
- **Explained:** [metric movement that maps to a known event — launch, iOS block, ETHDenver spike, feature ship]
- **Unexplained:** [metric movement with no obvious cause — this is where product insights live, flag for investigation]

Skip this subsection if all movements are within normal range and explained.

## Design

**Active:**
- [item] — [status]

**Handed to eng:**
- [item] — [eng owner]

**Waiting on design (eng blocked):**
- [item] — [sprint]

**Decisions needed:**
- [item] — [context]

## Convergent Signals
[Output of the convergent signals pass from Step 3. Each entry connects 2+ signals from different sections into one story. If no signals converge, skip this section.]

- [Signal A + Signal B] → [what they mean together, and what to do about it]

## Support Pulse (Intercom)
| Metric | Count |
|---|---|
| New conversations | |
| Bot-resolved | |
| Human-escalated | |
| Still open | |

**Top issues:**
- [Theme] — [count] — [resolution status]

**Cross-reference:**
- [Issue theme] → [Linear/Monday ticket ID if exists] — [ticket status vs user reports: aligned or diverged?]
- [Issue theme with NO ticket] → untracked, needs ticket or conscious ignore

[Flag open conversations needing follow-up. Surface product signals.]

## Feedback Pulse
[New feedback or worsening trends. Skip if nothing changed.]

- [Theme] — [X mentions, trending up/stable] — [implication]

**Cross-reference:**
- [Theme that maps to an active roadmap item]: confirms [roadmap item], evidence for prioritization
- [Theme that maps to a "not doing" item]: [X users] asking for something we cut. Re-evaluate or hold?
- [Theme with no roadmap/backlog item]: untracked. Needs a ticket or a conscious decision to ignore.

## Room Shifts
[Only if room files were refreshed and something changed.]

- [Group]: [what shifted] — [temperature trend: warming/cooling/stable over last 2 weeks]

Single-message shifts are noise. Only flag here if the shift is part of a multi-day trend visible across recent room file updates. Check the room file's history section, not just the latest refresh.

## Political Capital
**Wins (last 24h):**
- [concrete win — decision went {{YOUR_NAME}}'s way, credit received, influence extended]

**Losses:**
- [concrete loss — overruled, credit missed, influence diminished]

**Ledger trend:** [accumulating / spending / neutral]

Skip if nothing changed. When something does change, be specific about what happened, who was involved, and what it means for {{YOUR_NAME}}'s position. "Leadership presence is strong" is not useful. "{{FOUNDER_CEO}} cited {{YOUR_NAME}}'s reliability framing in the all-hands" is.

## Pipeline Follow-ups
[From `docs/users/users.md`. Skip if pipeline file doesn't exist or no follow-ups due.]

- [Contact] — [last interaction date] — [follow-up action due] — [channel]

## Coming Up (next 2-3 days)

- **[Day]: [event]**
  - Prep: [what specifically needs doing — pull data, frame narrative, draft slides]
  - Stakeholder concern to preempt: [if applicable]
  - Time estimate: [Xh]
  - Do today by: [time, if deadline-sensitive]

## Moves
[1-3 specific actions. Every move has: what to do, what it creates, and by when. No move without a mechanism and a deadline.]

- **Do:** [action] → **Creates:** [outcome] → **By:** [when]

## Energy Audit
[Only when kill list items have carried over 3+ days or total open commitments exceed what's realistic for today.]

- **Overcommitted on:** [area where obligations are accumulating]
- **Drop:** [item to cut, with reasoning]
- **Delegate:** [item to hand off, with who should take it]

Skip this section if the load is manageable. When it appears, it's a signal to subtract before adding.
```

## Weekly Output Format

When in weekly mode, use this format instead. It includes all daily sections plus the weekly-only additions.

```markdown
# Weekly Brief — [Month DD]

**Period:** [start date] to [end date]
**Sources scanned:** Linear ✓/✗, Monday ✓/✗, Slack ✓/✗, Signal ✓/✗, Mixpanel ✓/✗, Intercom ✓/✗, Apple Notes ✓/✗

## Summary
[3-5 sentences. The two-week arc. What's the story of these two weeks?]

## Kill List
[Same as daily format]

## Workstreams
[Same as daily format (10 roadmap themes), but flag 2-week color trends, not just yesterday's change]

## What Shipped (2 weeks)
- [Thing] — [date] — [source + link]

## What's In Progress
| Item | Theme | Owner | Status | Days in progress | Notes |
|---|---|---|---|---|---|

## What Didn't Move (should have)
[Same as daily "what didn't move" but with a 2-week lens. Items in progress 10+ days with no visible activity.]

## Decisions Made (2 weeks)
[Rolled up from daily decisions sections]

## Blocking / Blocked
[Same as daily format]

## Design (2-week view)

**Shipped (design complete, handed to eng):**
- [item] — [date]

**In progress:**
- [item] — [status]

**Waiting on design (eng blocked):**
- [item] — [sprint] — [days waiting]

**Decisions made:**
- [item] — [decision] — [date]

**Decisions pending:**
- [item] — [context]

## Metrics (2-week view)
| Metric | 2 weeks ago | Last week | Now | Trend |
|---|---|---|---|---|

[Include all daily metric tables plus the trend column. Same "What's the story" subsection.]

## Trends
- **Velocity:** [speeding up / slowing down / steady]
- **Top blocker pattern:** [recurring theme]
- **Feedback trend:** [improving / worsening / stable]
- **Support:** Open [n], Bot-resolved [n], Human [n], Trend [up/down/flat]
- **OKR trajectory:** [which KRs on track, which falling behind]

## OKRs (2-week view)
| KR | Target | Current | 2 weeks ago | Pace | On track? |
|---|---|---|---|---|---|

**Projection:** At current pace, which KRs hit target by EOQ? Which miss, and by how much?

**What moved the needle:** [which work items or events drove KR progress this period]

**What would move it next:** [specific actions that would close the biggest KR gaps]

## Stale (no movement 2+ weeks)
| Item | Theme | Owner | Last update |
|---|---|---|---|

## Convergent Signals
[Same as daily]

## Support Pulse (Intercom)
[Same as daily, with 2-week trend]

## Feedback Pulse
[Same as daily, with 2-week trend]

## Dependency Map
[Same format as status skill]

## Untracked Commitments
| Commitment | Who | Where | Date | Still open? |
|---|---|---|---|---|

## Room Shifts
[Same as daily, but 2-week temperature trend is mandatory, not optional]

## Political Capital
[Same as daily format, but cover the full 2-week period. Ledger trend is the important part here.]

## Pipeline Follow-ups
[Same as daily]

## Next Week
**Ship targets:**
- [What should ship next week]

**Unblock:**
- [What needs to get unstuck]

**Prep:**
- [Meetings/events that need preparation]

**OKR focus:**
- [Which KRs need targeted work]

**{{YOUR_NAME}}'s priorities:**
1. [Top priority for next week]
2. [Second]
3. [Third]

## Moves
[Same as daily]

## Energy Audit
[Same as daily]

```

## Rules

### File Management
- **One file per day.** If `cache/brief/brief-[YYYY-MM-DD].md` already exists, update it in place. Don't create a new file. Add an `## Updated [HH:MM]` section at the top with what changed so {{YOUR_NAME}} can see the diff at a glance.

### Accuracy
- **Never report something as done unless a source confirms it.** PR merged, issue closed, Slack message saying "shipped."
- **Never report something as blocked unless a source says so.**
- **Every item must trace to a source with a link.** Slack permalink, Linear URL, Monday URL, Google Doc link, Apple Note title. If no link, cite specifically (channel + person + date).
- **Flag stale data.** If a source couldn't be refreshed, say so.

### Completeness
- **Data must be fresh.** If caches are stale, refresh them before proceeding.
- **Complete but succinct.** If nothing changed in a category, omit it. Don't write "No updates" sections.
- **Kill List comes from todos, not from scratch.** Read existing todos, rank by leverage (who's waiting × what it unblocks × whether delay compounds), surface the top 4-6. Items carried over 3+ days move to "Drop or delegate."

### Updates
- **Todos get updated.** Move completed items to done.md. Add new action items from scans.
- **Feedback cache gets updated.** New feedback or status changes → `cache/feedback-monday.md`.
- **Room files get updated.** New messages → run read-room.
- **Save every daily.** The weekly mode reads them.

### Resilience
- **If a source fails, retry twice.** If still failing, inform {{YOUR_NAME}} ("Linear scan failed after 2 retries: [error]") and note it in the output file under Sources Refreshed as ✗ with the reason. Continue with available data.
- **Strategic signals come from cross-referencing.** Three bug reports about the same flow is a strategic signal. A competitor mention contradicting a roadmap assumption is a strategic signal.

### Weekly-Specific Rules
- **Two-week window is mandatory.** Patterns only emerge over 2 weeks.
- **Daily briefs are the primary input.** Read all briefs from the period before scanning fresh data. They're the foundation.
- **Next week plan is not optional.** The look-ahead is half the value.
- **Trends matter more than snapshots.** A single blocked item is a status fact. The same item blocked for 2 weeks is a trend.
- **OKR trajectory is required.** Project current pace against EOQ targets. Flag which KRs will miss.
- **After weekly completes, run comms.** The weekly brief produces the data. The comms skill (Monday Company Update type) drafts the post. Always run comms after weekly.

## Trigger

**Daily:** `/daily-brief`, `/daily`, "morning briefing", "daily briefing", "what's happening today"
**Weekly:** `/weekly-brief`, `/weekly`, "draft weekly update", "weekly company update", "write the weekly", "weekly brief"
