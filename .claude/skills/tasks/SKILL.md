---
name: tasks
description: Organize todos and surface urgent work
---

# Task Orchestrator

## Purpose

Surface all open todos from scattered sources, organize them into a scannable Kill List by day, help get them done.

**Core principle:** Most task lists fail because they're too long. This skill creates a daily Kill List (4-6 items) + quick batch tasks, then organizes everything else into a Parking Lot table. Focus on what blocks the current milestone or blocks other people.

## Reads
- `.claude/skills/utils/sources.md` — cache freshness rules, scanning instructions
- `cache/tasks/tasks-db.md` — current tasks
- `cache/tasks/done.md` — completed tasks
- `cache/` — all relevant caches (Slack, Linear, Monday, Apple Notes, GDocs, GSheets)
- `context/current-context.md` — decision-making context (why things matter, how to prioritize)
- `context/okr-status.md` — OKR targets (prioritize tasks that advance red/yellow KRs)
- `context/org-dynamics.md` — political context (flag when a task has political implications)
- `context/cadences.md` — deadlines, what's due when
- `docs/product/master-shield-prd-roadmap.md` — master PRD and roadmap (source of truth)
- `docs/strategy/` — priorities
- `docs/feedback/` — user input

## Writes
- `cache/tasks/tasks-db.md` — organized kill list, parking lot (overwritten each run)
- `cache/tasks/todos-[YYYY-MM-DD].md` — dated copy for history
- `cache/tasks/done.md` — completed task archive

## Input Sources

All external sources are read from caches per `.claude/skills/utils/sources.md`. Tasks never scans MCPs directly — it reads cached data and filters for {{YOUR_NAME}}-relevant items (assigned to him, mentions him, blocks him, or he's waiting on).

1. **`cache/tasks/tasks-db.md`** — existing task list
2. **`cache/tasks/done.md`** — completed tasks (to exclude)
3. **Slack cache** (`cache/slack.md`) — action items mentioning {{YOUR_NAME}}, blockers where he's named, commitments he made
4. **Linear cache** (`cache/linear.md`) — issues assigned to {{YOUR_NAME}} or where he's blocking
5. **Monday cache** (`cache/monday.md`) — items assigned to {{YOUR_NAME}}
6. **Apple Notes cache** (`cache/apple-notes.md`) — random jots, quick captures
7. **Google Docs cache** (`cache/gdocs.md`) — action items from meeting notes mentioning {{YOUR_NAME}}
8. **Monday Feedback cache** (`cache/feedback-monday.md`) — feedback items with action required

If a cache is stale, refresh it per `sources.md` before proceeding. **Freshness must be checked by comparing the cache file's `Last refreshed` timestamp against the current time and the source's threshold in `sources.md`. Never assume a cache is fresh because it was refreshed earlier in the session — that refresh may have been for a different skill or time window. Always use absolute timestamps.** Do NOT present a kill list built on stale data. Refresh first, then organize.

**When caches are unavailable:** Don't skip prioritization. Fall back to leadership context to prioritize and plan:
- `context/cadences.md` — what's due, what meetings are coming
- `context/current-context.md` — what matters right now, implicit priorities
- `context/okr-status.md` — which KRs are red/yellow, what needs work
- `context/org-dynamics.md` — political implications of task ordering

These files are always available locally. Even without fresh MCP data, tasks can produce a useful kill list by prioritizing against deadlines, OKR gaps, and political context.

## Urgency Keywords

Parse these from notes to determine priority:

| Keyword                                                                     | Priority     |
| --------------------------------------------------------------------------- | ------------ |
| `URGENT` `ASAP` `BLOCKING` `blocks` `NOW`                                   | 🔴 Urgent    |
| Day names (`monday`, `thursday`), dates (`jan 20`), `tomorrow`, `this week` | 🟠 Important |
| `later` `someday` `backlog` `eventually` `low`                              | ⚪ Later      |
| No keyword                                                                  | 🟡 Normal    |

Also infer urgency from context:

- "[person] waiting" → 🔴
- "blocks [thing]" → 🔴
- "due [date]" → check date, assign accordingly

## Process

1. Read `cache/tasks/done.md` — load completed items to exclude from results
2. Read existing `cache/tasks/tasks-db.md` — move any `[x]` items to `cache/tasks/done.md` with today's date
3. Check `Last refreshed` timestamp on each cache in `cache/`. If any cache is older than 24 hours, run the sources scan (`.claude/skills/utils/sources.md` Parallel Scan Protocol) to refresh stale caches before proceeding. Each agent writes directly to its cache file and returns only a status message (success/error/stuck). Do NOT have agents return full data. Note which caches were fresh vs refreshed vs unavailable. Cache files: `slack.md`, `linear.md`, `monday.md`, `apple-notes.md`, `gdocs.md`, `feedback-monday.md` (all in `cache/`).
4. Read all caches, filter for {{YOUR_NAME}}-relevant items (assigned to him, mentions him, blocks him, he committed to it). Pull all open items with dates:
   - Apple Notes: use note creation/modification date
   - Linear: use issue `createdAt` date
   - Monday.com: use item creation date
   - Slack: use message timestamp
   - Google Docs: use comment/doc creation date
   - If date unavailable, estimate from context or mark as "unknown"
5. Parse urgency keywords
6. Deduplicate across sources
7. Identify dependencies — who's waiting on me, who I'm waiting on
8. **Organize into Kill List:**
   - Start with TODAY — max 4-6 focused tasks
   - Then each upcoming day in current week/sprint
   - Separate "quick batch" micro-tasks (under 5min each)
   - Everything else → Parking Lot table
9. **Apply the filter:**
   - Kill List = blocks launch/milestone OR blocks a person
   - I'm Blocking = tasks blocking other people (unblock first)
   - Waiting On = can't do, waiting on others
   - Parking Lot = everything else, in table format, ordered by Priority → When
10. **Separate true backlog/roadmap items:**
    - Features with no timeline (custom tokens, Ledger, etc.) → mention moving to roadmap doc
    - Roadmap ideas → note in Parking Lot but flag for roadmap skill
11. Output in format below
12. **VALIDATION CHECK (Pre-Display):**
    - Verify NO verbose separated format with dashed lines (────)
    - Verify tables used for: I'm Blocking, Waiting On, Known Issues
    - Verify Parking Lot is condensed bullet format (NOT table in output)
    - Verify Kill List uses numbered lists with context
    - If any format violation found, reformat before displaying
13. Update `cache/tasks/tasks-db.md` with organized Kill List format
14. Save dated copy to `cache/tasks/todos-[YYYY-MM-DD].md`

## Storage

- **Primary task list**: `cache/tasks/tasks-db.md` — open items only
- **Completed archive**: `cache/tasks/done.md` — memory of finished tasks
- After running:
  1. Read `cache/tasks/done.md` first — these items should NOT be re-added
  2. Move any `[x]` items from `cache/tasks/tasks-db.md` → `cache/tasks/done.md` (with completion date)
  3. Update `cache/tasks/tasks-db.md` with consolidated Kill List + Parking Lot table
- Deduplication: if task exists in `cache/tasks/done.md`, skip it even if source still has it

## Output Format

**ALWAYS organize tasks in this format for maximum scannability:**

### 🎯 KILL LIST — [Current Week/Period]

Organize by day with 4-6 focused tasks per day. Start with today, then each upcoming day.

**[Day of Week, Date]**

1. **Task name** — context (why urgent/who's waiting)
2. **Task name** — context
3. **Task name** — context
4. **Task name** — context
5. **Task name** — context

**CRITICAL: Kill List items MUST be numbered (1, 2, 3...) so user can mark complete by number (e.g., "done 1, 2, 3").**

**Quick batch (Xmin):**
- Micro-task 1
- Micro-task 2
- Micro-task 3

*Quick batch uses bullets since typically completed as a group ("batch done")*

---

### 🚨 I'M BLOCKING

| Person | Task | Since |
|--------|------|-------|

---

### ⏳ WAITING ON (Check Friday)

| Person | What | Since |
|--------|------|-------|

---

### 🐛 KNOWN ISSUES

| Bug | Status | Since |
|-----|--------|-------|

---

### 📅 DAILY RULES

**Every morning (5min):**
1. Check "I'm Blocking" — unblock blockers first
2. Glance at Kill List for today
3. Any meetings today? Flag ones worth prepping (use comms → Meeting Prep)
4. Ignore everything else

**Every evening (2min):**
1. Mark what got done
2. Move 1-2 items forward to tomorrow if critical
3. Everything else stays in Parking Lot

**Every Friday:**
- Review full Parking Lot in tasks-db.md to catch miscategorized items

**If something urgent comes up:**
- Ask: "Does this block [milestone] or block a person?"
- No → Parking Lot
- Yes → bump lowest priority item from today's Kill List

---

### 📋 PARKING LOT (Review for Miscategorized Items)

**CRITICAL: ALWAYS use condensed bullet format for display. NEVER show as table in output.**

**Format rules:**
- Show top 10-15 "This Week" items as bullets: `- Task name (Category)`
- Group by "When" (This Week, Post-Launch, Q1, Later)
- Count total items: `[View full Parking Lot in tasks-db.md — X total items]`
- Full table stays in tasks-db.md file for Friday review
- NEVER use verbose format with field-by-field breakdowns and dashed separators

**Example output:**

📋 PARKING LOT (Review for Miscategorized Items)

**This Week (15 items):**
- Clean analytics of PII (Launch Prep)
- Analytics for flows (Analytics)
- Send Leo analytics to Slack (Analytics)
- Update OKRs → Kyle (Leadership)
- {{PRODUCT}} feemaster runbook (Team)
- Ambassadors (ETHDenver)
- Scratch cards (ETHDenver)
- Booth staffing (ETHDenver)

**Post-Launch (20 items):**
- Analytics for onboarding (Analytics)
- Contact Us in help section (Product)
- Support runbook (Documentation)

**Q1 (28 items):**
- Wallet vision (Product)
- Privacy space analysis (Product)
- Positioning (GTM)

**Later (2 items):**
- Reach out to eng team from Dapper (Hiring)

**[View full Parking Lot in tasks-db.md — 65 total items]**

**Weekly reminder:** Review full Parking Lot every Friday to catch miscategorized items.

**When options:**
- This Week
- Post-Launch (or whatever the next milestone is)
- Q1 / Q2 / etc.
- Later (no clear timeline)

**Priority options:**
- 🔴 Urgent
- 🟠 Important
- 🟢 Post-Launch (milestone-specific)
- 🟡 Normal
- ⚪ Later

---

### Sources Checked

List which sources were accessed and which were skipped/unavailable at the bottom for reference.

---

## done.md Format

When moving completed items to `cache/tasks/done.md`:

1. **Sort by completion date** (newest first)
2. **Group by date header** (e.g., `### Jan 31`)
3. **Single table per date** with columns:

| Task | Category | Priority | Source | Added | Completed |
|------|----------|----------|--------|-------|-----------|

4. **Categories**: Leadership & Comms, Launch Prep, Team & Coordination, Analytics & Infra, Product & Design, Extension & Mobile, Partnerships, GTM & Marketing, ETHDenver Prep, Documentation & Process, Technical, People Follow-ups, Testing, Backlog

5. **Priority values**: 🔴 Urgent, 🟠 Important, 🟡 Normal, ⚪ Later

6. **Stats section** at bottom:
```
## Stats
| Metric | Value |
|--------|-------|
| Total completed | X |
| 🔴 Urgent | X |
| 🟠 Important | X |
| 🟡 Normal | X |
| Oldest task age | X |
```

7. **Update timestamp** at end: `*Updated: YYYY-MM-DD*`

---

## Deep Prioritize Mode

**Trigger:** "prioritize [list]", "what should I do first", "prioritize against [Google Sheet URL]"

Goes beyond the daily Kill List. Cross-references todos against project plans, roadmaps, and strategy docs to find what's missing and what doesn't belong.

### Process

1. **Gather** — Read tasks-db.md, relevant Google Sheets (launch tracker, project plan, OKRs), strategy docs, feedback summaries
2. **Cross-reference** — Compare todos against project plan. Find:
   - **Gaps**: In the plan but NOT in todos
   - **Orphans**: In todos but NOT tied to any plan
   - Flag dependencies and blockers, flag items needing owners or scope
3. **Prioritize** — Apply framework to the full list including gaps

### Frameworks

**Default: Impact x Urgency**
- Do Now: High impact + urgent (blocking others, deadline, high value)
- Do Soon: High impact + not urgent
- Schedule: Low impact + urgent (delegate or timebox)
- Backlog: Low impact + not urgent

**Alternatives (use when specified):** ICE (Impact x Confidence x Ease), RICE (Reach x Impact x Confidence / Effort), MoSCoW, Dependencies-first (what unblocks the most other work)

### Output Additions

On top of the standard Kill List output, Deep Prioritize adds:

**Top 3 Right Now**
1. [Item] — [why it's #1]
2. [Item] — [why]
3. [Item] — [why]

**Gaps Identified** — Items in project plan but missing from active todos:
- [ ] [Item] — [source sheet] — [owner needed?]

**Orphans** — Items in todos but not tied to any project plan:
- [ ] [Item] — [recommend: add to plan / drop / clarify]

**Dependencies Map**
- [Item A] → blocks → [Item B, Item C]
- [Item D] → waiting on → [Person/External]

---

## Rules

- **One file per day.** If `cache/tasks/todos-[YYYY-MM-DD].md` already exists, update it in place. Don't create a new file. Add an `## Updated [HH:MM]` section at the top with what changed so {{YOUR_NAME}} can see the diff at a glance.
- NEVER skip "I'm Blocking" or "Waiting On" — ask me if unclear
- ALWAYS state which sources were checked
- If a source is unavailable, say why
- Dedupe: if same task appears in multiple sources, show once, note all sources
- After output, ask if I want to update tasks-db.md
- **Parking Lot goes LAST** — it's the safety net to catch miscategorized items
- **Separate roadmap from todos** — true backlog features belong in roadmap, not todos

## Format Validation (Pre-Display Checklist)

Before displaying output to user, verify:

1. ✅ **No verbose separated format** — no field-by-field breakdowns with `────` separators
2. ✅ **Tables for:** I'm Blocking, Waiting On, Known Issues (markdown table format)
3. ✅ **Parking Lot:** Condensed bullet format grouped by "When", NOT table
4. ✅ **Kill List:** MUST use numbered list (1, 2, 3...) so user can reference by number to mark complete
5. ✅ **Quick batch:** Bullet points with time estimate (users say "batch done")

If ANY violation found, reformat before displaying. User must NEVER see verbose separated format.

## Execution Help

After showing todos, I can:

- Draft responses (Slack, email)
- Create tickets (Linear, Monday)
- Write docs or briefs
- Break down large tasks
- Mark items done and update tasks-db.md

### Marking Tasks Complete

User can mark tasks done using shorthand:
- "done 1, 2, 3" (by Kill List number)
- "✓ PII, ✓ Houdini" (by keyword)
- "batch done" (all quick batch tasks)
- "first 3 done" (tasks 1-3)
- "x1 x2 x4" (x for done)

When user marks tasks complete:
1. Identify which tasks from today's Kill List
2. Move to done.md with completion date and metadata
3. Update tasks-db.md to remove completed items
4. Show remaining tasks for today

## Trigger

"what's urgent" / "show my todos" / "run tasks" / "prioritize [list]" / "what should I do first" / "prioritize against [Google Sheet URL]"
