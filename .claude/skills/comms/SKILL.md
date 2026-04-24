---
name: comms
description: Create leadership and investor updates
---

# Comms Skill

## Purpose
Create communications that position {{YOUR_NAME}} as the product leader driving {{PRODUCT}} forward. Every comm should align stakeholder interests, champion the right narratives, and make {{YOUR_NAME}}'s impact visible across engineering, BD, marketing, leadership, and founders.

**Default behavior: Ask {{YOUR_NAME}} when unclear.** Tone, framing, audience, narrative angle, strategic intent, who to credit, what to emphasize, what to leave out. Prompting {{YOUR_NAME}} leads to a result that fits. Guessing leads to rewrites. Ask early, ask often.

## Dependencies
- `.claude/skills/utils/voice.md` — apply before all output

## Reads
- `context/current-context.md` — decision-making context (what to emphasize, why things matter)
- `context/narrative-map.md` — active narratives, what to push, gaps (mandatory for Monday comms)
- `context/okr-status.md` — OKR targets vs actuals (frame progress against goals)
- `context/org-dynamics.md` — political context (stakeholder alignment, what lands with whom)
- `context/cadences.md` — what's due, timing context
- `context/room/` — room analysis (who's who, influence, temperature, positions)
- `context/room/people/<name>.md` — person profiles (what persuades them, how they see {{YOUR_NAME}})
- `docs/strategy/` — strategic context
- `docs/narrative/` — messaging, positioning
- `docs/product/` — what shipped
- `docs/product/master-shield-prd-roadmap.md` — master PRD and roadmap (source of truth)
- `tasks-db.md` — current tasks
- `done.md` — completed tasks

## Writes
- `cache/comms/` — leadership updates, digests

## Core Principles

### 1. Narrative Tracking
Maintain awareness of narratives {{YOUR_NAME}} should champion internally. Before drafting any comm, check which narratives are active and weave them in naturally.

**Narrative categories:**
- **Product narratives**: What makes {{PRODUCT}} matter (privacy as the core need, convenience over speed, multi-asset privacy, etc.)
- **Strategic narratives**: Where {{PRODUCT}} sits in the market and where it's going (competitive positioning, ecosystem plays, partnership angles)
- **Organizational narratives**: What {{YOUR_NAME}}'s team is delivering and why it matters to the company (launch readiness, cross-functional execution, user traction)

**When narratives shift or new ones emerge, ask {{YOUR_NAME}} before adopting them.** Don't assume. Narratives are strategic choices.

### 2. Stakeholder Alignment
Every comm should be written with all stakeholders in mind, not just the direct recipient. The same update lands differently depending on who reads it.

**Load the room file.** Before drafting any comm that goes to a group or has multiple readers, load the relevant `context/room/<group-name>.md` from the read-room skill. That file tells you who carries weight, what each person cares about, where tensions exist, and how the room will read your message. If the room file is stale or missing, run `/read-room` first.

The room file replaces static stakeholder guesses. Use it for: who to address, what framing lands with whom, what concerns to preempt, and what tone the room expects.

### 3. Positioning {{YOUR_NAME}} as Leader
Comms should demonstrate leadership, not claim it. Techniques:

- **Show cross-functional coordination**: "After syncing with engineering on X and marketing on Y, we're doing Z"
- **Own decisions**: "I've decided to..." not "We're thinking about..."
- **Anticipate problems**: Surface risks before they're asked about
- **Credit the team, own the strategy**: "{{ENG_LEAD}}'s team shipped X, which puts us in position to Y"
- **Connect tactical wins to strategic outcomes**: Never just report status. Say why it matters.
- **Frame blockers as decisions you're driving**: Not "we're blocked on X" but "I'm pushing for a decision on X by Friday"

## Input Sources
1. **Slack Digest**: `cache/comms/shield-slack-digest-*.md` for recent activity
2. **Project Status**: Latest status from todos, roadmap
3. **Feedback Engine Output**: Key feedback themes
4. **Strategic Radar Output**: Market context, competitor moves
5. **Google Docs**: Meeting notes, decision logs
6. **Launch Tracker**: Milestones and dates
7. **Narrative tracker**: Active narratives (see below)

## Process

### Step 1: Identify Audience and Purpose
- Who is this for?
- What do they need to know, decide, or do?
- Which narratives should this comm reinforce?

**If any of these aren't obvious, ask {{YOUR_NAME}}.** Don't infer audience or purpose from context alone. A quick "Who's this for and what's the goal?" saves a full rewrite.

### Step 2: Check Active Narratives
Search `docs/` for `*narrative*.md`, `*strategy*.md`, `*positioning*.md`. Identify which narratives are currently active and relevant to this comm.

**If unsure which narrative angle to take, ask {{YOUR_NAME}}.** Don't guess on strategic framing.

### Step 3: Gather Context
Pull from input sources. For each fact or update, think about how it maps to each stakeholder's interests.

### Step 4: Draft
Write with these rules:
- **Voice**: Direct, confident, conversational. Product lead talking to peers and leadership. Not formal, not casual. "We" not company names internally.
- **No LLM patterns**: No "The key insight is...", no "Let's break this down...", no hedge words.
- **No em dashes.** Use periods or commas instead.
- **No sentence fragments as emphasis.** Every sentence carries meaning and connects to the next. "Full privacy, no effort." is empty. Turn it into a thought that moves the narrative forward.
- **Lead with impact, not activity.** "{{PRODUCT}} is now live on 3 platforms" not "We submitted builds to 3 app stores."
- **Every update should answer "so what?"** Don't just report. Connect to outcomes.
- **When in doubt, ask {{YOUR_NAME}}.** Better to pause than to send the wrong message.

**Narrative rules for comms:**

- **Each paragraph flows from the one before.** No jumps. The reader should never wonder "why are we talking about this now?" Each paragraph adds more context to what came before it.
- **Insight lines are connective tissue.** A single line of philosophy or context can do triple duty: reframe bad news that just landed, set up what's coming next, and bridge two otherwise disconnected subjects under a shared frame. Example: "Shots at the goal are more important than perfection" looks back (Apple situation isn't a crisis), looks forward (imperfections are expected), and connects the logistics paragraph to the product paragraph. Without it the reader feels the seam between topics. With it, the whole message feels like one conversation.
- **Bad news never lands without context that gives it meaning.** The context can come before, after, or both. The reader should never sit with a problem and no frame for how to think about it.
- **One strong metaphor carries the message.** Don't stack metaphors. Pick one and commit. Let it do the work.
- **Weave limitations in stride.** Don't create a "Known Issues" section. Mention them naturally inside the flow.
- **Spirit of an idea, not the literal statement.** If {{YOUR_NAME}} says "don't say this directly but [idea]," weave the feeling in. Don't restate the words.
- **Prose, not structure, for Slack and broad comms.** No headers, no bullet lists, no tables. Flowing paragraphs. Headers and bullets are for PRDs and status reports, not messages to the room.
- **Short.** 150 words beats 400. If someone would skim it, it's too long. Every paragraph earns its place.
- **End with energy, not a summary.** No "in conclusion" or structured next-steps. Close with forward momentum.
- **Don't over-explain to internal audiences.** They built it. They know what the infrastructure does.

### PM Vocabulary (for internal comms)

Internal comms should use precise product management language. This vocabulary signals leadership and structural thinking to stakeholders. (Note: this is the opposite of X content, where builder language replaces PM language. Internally, PM vocabulary is a feature.)

| Term | When to use |
|---|---|
| PRD | Referencing product specs or requirements docs |
| Stakeholder alignment | When multiple parties need to agree before moving forward |
| Cross-functional | Work spanning engineering, design, BD, marketing |
| OKRs / KPIs | Referencing goals, targets, measurable outcomes |
| Roadmap prioritization | Deciding what to build next and why |
| Sprint | Time-boxed work cycles, planning, retros |
| User research / synthesis | Formal feedback analysis, interview findings |
| Facilitated | Led a meeting, workshop, or decision process |
| Managed up | Ensured leadership visibility on a risk or decision |

### Step 5: Align Check
Before finalizing, re-read the room file. Scan the draft through each person's lens: would they find this useful, would they push back, would they feel their perspective was considered? If anyone who matters would read this and think "this doesn't help me" or "this misreads the situation," adjust.

**If the comm could be read multiple ways or the stakes are high, show {{YOUR_NAME}} the draft and ask specific questions** ("Should this emphasize X or Y?", "Is this the right level of detail for {{FOUNDER_CEO}}?", "Do you want to name the blocker or keep it vague?"). Iteration with {{YOUR_NAME}} produces better comms than trying to nail it in one shot.

## Comm Types

### Leadership Update ({{FOUNDER_CEO}}, {{COO}})
**To**: [Recipients]
**Subject**: {{PRODUCT}} Update, [Date]

**TL;DR**: [1-2 sentences. Impact-first.]

**What shipped**
- [Win 1] — [why it matters]
- [Win 2] — [why it matters]

**Risks I'm managing**
- [Risk] — [what I'm doing about it, what I need]

**Decisions needed**
- [Decision] — [recommendation, deadline]

**Next week**
- [Focus areas with clear outcomes]

---

### Team Announcement (#channel)
**Channel**: #[channel]

[Lead with the win. Credit who made it happen. Connect to the bigger picture. Clear next steps if any.]

---

### Cross-Functional Update (engineering + BD + marketing)
**TL;DR**: [1 sentence]

**For engineering**: [What this means for your work]
**For BD/partnerships**: [What this enables in conversations]
**For marketing**: [Narrative hook, timing, content angle]

**What's next**: [Clear priorities]

---

### Investor / Board Update
**Subject**: {{PRODUCT}}, [Month] Update

**Highlights**
- [Metric/milestone with context]
- [Competitive positioning move]

**Product momentum**
[2-3 sentences connecting progress to vision]

**Market context**
[What's happening in the space that validates our direction]

**What's next**
[Upcoming milestones]

**Ask**
[If any]

---

### Executive Roadmap Summary ({{FOUNDER_CEO}}, {{COO}}, Board)

90-second briefing format. Takes a roadmap and distills it into what leadership actually needs: outcomes, impact, and risk. Pull from `docs/product/master-shield-prd-roadmap.md` and `docs/strategy/`.

```markdown
## Q[X]: [Theme]
[One compelling sentence about the outcome]

**What we're building:** [3-4 initiatives max, one line each]

**Why it matters:** [Pick one: revenue impact, user retention, or competitive moat. Be specific.]

**Biggest risk:** [What could kill this? Be honest.]
```

Repeat per quarter. No fluff, no buzzwords, just impact. Write like you're briefing a CEO who has 90 seconds.

---

### Meeting Prep / Objection Handler

Pre-meeting stress test. Simulates each stakeholder's reaction to a proposal so you walk in prepared, not blindsided.

**Input needed:**
1. What you're pitching (proposal, feature, roadmap change, resource ask)
2. Who's in the room (names and channel/group if applicable)

**Process:**
Load the relevant room file(s) from `context/room/<group-name>.md` for anyone in the meeting. Use their profiles, authority levels, motivations, and position history to answer:
- **Why would they hate this?** Be specific to their actual positions and incentives from the room file.
- **What question will they ask that you're not prepared for?**
- **What would make them say yes?** Use what's persuaded them before (tracked in the room file).

Then provide a 1-sentence reframe for each objection that flips it to your advantage.

**Output format:**

```markdown
## [Stakeholder Name] — [Role]

**Their incentive:** [What they're optimizing for]

**They'll push back on:** [Specific objection tied to their KPIs/politics]

**The question you're not ready for:** "[Question]"

**What makes them say yes:** [The framing that aligns your proposal with their goals]

**Reframe:** "[1-sentence flip that turns the objection into a reason to support]"
```

Be brutally honest. Better to get roasted here than blindsided in the room.

---

### Monday Company Update (weekly Slack post)

Company-wide weekly update. Run `/weekly` first to get the data, then draft the post from the weekly brief.

**Read before writing:** `docs/daily/weekly-*.md` (most recent), `context/narrative-map.md`, `context/org-dynamics.md`, `skills/comms.md` narrative rules.

**Rules:**
- Prose paragraphs, no headers, no bullet lists. Flows like talking to the room.
- Each paragraph flows from the one before. No jumps.
- One strong metaphor carries the message if needed.
- Bad news never lands without context that gives it meaning.
- Weave limitations in stride, don't spotlight them.
- "We" not company names.
- Short. 200-300 words. Every paragraph earns its place.
- End with energy, not a summary.
- Credit the team, own the strategy.
- Lead with impact, not activity.

**Structure (invisible to reader, visible to writer):**
1. Open with the week's biggest win or most important shift.
2. Connect it to what it means.
3. Acknowledge what's hard or stuck, framed with context.
4. Close with forward momentum. What's next, what we're aiming at.

**Output:** `cache/comms/monday-[YYYY-MM-DD].md`

**Channel:** company-wide Slack
**Audience:** full team (eng, design, BD, marketing, leadership)

Cross-reference against `context/narrative-map.md`: which narratives does this post reinforce? Are there narratives we should be pushing that aren't represented?

**Never send without {{YOUR_NAME}}'s approval.** Show the draft and ask: is the framing right? Anything to add, cut, or reframe? Any political considerations?

---

### Slack Message (quick update, thread reply, announcement)
Write as {{YOUR_NAME}} would. Short, direct, no fluff. Match the channel's energy. Lead with the point.

---

### Status Report (Workstream Summary)

Weekly cross-workstream snapshot. Pulls from project skill inputs (Linear, Monday.com, Slack, testing trackers) plus todos/done/roadmap.

**Overall Status**: On Track / At Risk / Blocked

**Summary**: [2-3 sentences on where things stand]

**Progress by Workstream**

| Workstream | Status | Update | Blockers |
|---|---|---|---|
| Extension | On Track / At Risk / Blocked | | |
| Mobile | On Track / At Risk / Blocked | | |
| Infra | On Track / At Risk / Blocked | | |
| Design | On Track / At Risk / Blocked | | |
| QA | On Track / At Risk / Blocked | | |
| Partnerships | On Track / At Risk / Blocked | | |
| Legal | On Track / At Risk / Blocked | | |
| GTM | On Track / At Risk / Blocked | | |

**Key Wins**
- [Win] — [why it matters]

**Blockers & Risks**
- [Blocker] — Owner: [Name] — ETA: [Date]

**Decisions Needed**
- [Decision] — needed by [Date]

**Next Week Focus**
- [Priority 1]
- [Priority 2]

**Output destinations**: Google Doc (weekly status doc), Slack to #shield-wallet-leadership, Email to {{FOUNDER_CEO}}/{{COO}} (if requested)

---

## Narrative Tracker

Active narratives live in `context/narrative-map.md`. Always read that file before drafting any comm. Don't maintain a separate copy here.

## Output Destinations
- Local file in `cache/comms/` (always)
- Google Doc (if updating an existing doc)
- Slack (if approved by {{YOUR_NAME}})
- Email (if requested)

## Trigger
`/comms`, "draft update for [audience]", "write a message for [channel]", "leadership update", "run project-status", "generate status report", "status report", "executive roadmap summary", "summarize roadmap for execs", "prep me for [meeting]", "stress test this pitch", "objection handler"
