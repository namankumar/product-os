---
name: meeting-notes
description: Scan email for meeting notes, fetch transcripts, extract decisions
---

# Meeting Notes Skill

## Purpose

Scan email for auto-generated meeting notes, fetch full transcripts, extract what matters. Meetings are where decisions get made, commitments get spoken, and political dynamics play out. This skill captures all of that before it fades.

## Reads
- `.claude/skills/utils/sources.md` — scanning instructions for Gmail, Google Docs, Roam transcripts
- `context/meeting-notes.md` — prior meeting notes (diff against this)
- `context/current-context.md` — {{YOUR_NAME}}'s interpretive lens
- `context/org-dynamics.md` — existing political context (to detect shifts)
- `context/room/` — existing people profiles (to update)

## Writes

Each extracted item routes to the canonical file where it'll actually be read, not just dumped into meeting-notes.md.

| Content type | Destination |
|---|---|
| Raw extractions (all types) | `context/meeting-notes.md` — rolling cache |
| Commitments ({{YOUR_NAME}}'s) | `tasks-db.md` — open todos |
| Commitments (others') | `tasks-db.md` — "I'M BLOCKING" / watch list |
| People signals | `context/room/people/<name>.md` — updated, not overwritten |
| Political signals + interpretive reads | `context/org-dynamics.md` — flagged to {{YOUR_NAME}} before writing |
| Team instincts + decision rationale | `context/current-context.md` |
| Estimates (eng effort, capacity, numbers) | `context/current-context.md` |

## Process

### Step 1: Find New Meetings

Read `context/meeting-notes.md` first. Check the **Last refreshed** timestamp (stored as `YYYY-MM-DD HH:MM TZ`). Search Gmail for meeting notes *after* that timestamp — not just the date. The skill may run multiple times per day; date-only diffing causes re-processing of already-extracted meetings.

After filtering by timestamp, diff the email list against meetings already listed in the **Recent Meetings** table (by meeting name + date). Skip any meeting that already appears in the table. Only process new entries.

Three senders:
- `gemini-notes@google.com` — Gemini auto-notes
- `meetings-noreply@google.com` — Google Meet
- `roam@ro.am` — Roam Magic Minutes

### Step 2: Fetch Transcripts

For each new meeting:
- **Gemini meetings:** Always search Drive by meeting name: `search_docs(query="[Meeting Name]", user_google_email=...)`. Drive search returns **two docs** for most meetings — fetch both:
  1. `[Meeting Name] - YYYY/MM/DD HH:MM TZ - Notes by Gemini` — the auto-generated transcript. Always has more than the email summary.
  2. `[Meeting Name]` (no timestamp suffix) — the rolling agenda/notes doc attached to the calendar invite. Contains pre-prepared agenda items, OOO dates, host-written action items, and prior-meeting context that the transcript doesn't capture.
  Read both via `get_doc_as_markdown`. Extract from the combination. If the email body has the Doc ID directly, use it — but still search Drive afterward to find the agenda doc. **Never fall back to email summary if Drive search succeeds.**
- **Roam meetings:** Extract Magic Minutes share link from email, open with `agent-browser --session roam-scan --state ~/.agent-browser/roam-auth.json open <link>`. Click "Transcript", hover download button (top right), click "Copy", read clipboard with `eval "navigator.clipboard.readText()"`. Save to `cache/transcripts/`. Close session after.
- **Google Meet (no transcript):** Note as empty, skip extraction.
- **Last resort only:** Use Gemini email body as source of truth only if both the email link AND Drive search fail. Note it explicitly as email-summary-only.

### Step 3: Extract

From each transcript or email summary, extract:

**Decisions** — what was agreed, who agreed, what it changes. Always include *why* — the rationale is what makes a decision durable and reversible.

**Commitments** — who said they'd do what, by when. Highest-value extraction. Verbal commitments with no issue are the things that slip.

**Status changes** — something shipped, blocked, unblocked, or changed direction.

**Dependencies** — X is waiting on Y for Z.

**People signals** — how someone operates, what they care about, how they position themselves.

**Political context** — power dynamics, tension, alignment shifts, positioning.

**Team instincts** — notable opinions, team energy, resistance signals, analogies used. Not decisions, but they predict which decisions will stick and which will get quietly ignored. Capture the room's instinct even when it doesn't become a formal decision.

**Interpretive reads** — pattern inferences that don't trace to a single quote. What the meeting reveals about how someone is positioning, what they didn't say, what the attendee list itself signals. Label these as reads, not facts.

**Estimates** — quantified data: eng week estimates, user numbers, cost figures, timeline projections. Capture the number and the context.

**Sequence intelligence** — when multiple sessions happen the same day, the order matters. What the sequence reveals (a concession meeting before the main session, a decision already made before discussion started) is often more important than any single session's content.

### Step 4: Write

**Update `context/meeting-notes.md`** with the full extraction for the meeting.

**Propagate outward** — after each meeting extraction, route items to their canonical destinations:

- For each of {{YOUR_NAME}}'s commitments: propose a `tasks-db.md` entry
- For each others' commitment worth tracking: propose a `tasks-db.md` watch entry
- For each people signal: write to `context/room/people/<name>.md`
- For each political signal or interpretive read: surface to {{YOUR_NAME}}, then write to `context/org-dynamics.md`
- For team instincts, decision rationale, estimates: propose additions to `context/current-context.md`

Don't ask permission for people files — just update them. Ask before writing political signals to org-dynamics.

## Output Format

```markdown
# Meeting Notes

**Last refreshed:** [date]
**Scope:** [date range]

## Recent Meetings

| Date | Meeting | Attendees | Source | Link |
|---|---|---|---|---|

## Extractions

### [Meeting Name] — [Date]

**Decisions:**
- [what was decided] — *why: [rationale]*

**Commitments:**
- [who] will [what] by [when]

**Status changes:**
- [item/workstream] — [change]

**Dependencies:**
- [who] waiting on [who] for [what]

**Estimates:**
- [item] — [number/timeline] ([owner])

**Team instincts:**
- [topic] — [what the room's energy revealed, even if not a decision]

**People:**
- [name] — [signal]

**Political:**
- [dynamic observed]

**Read:**
- [interpretive inference — labeled as read, not fact]
```

When multiple sessions happen the same day, add a **Sequence** section after the individual extractions:

```markdown
### Sequence — [Date]

[What the order of sessions reveals that individual extractions don't. Who conceded before the main meeting started. What was already decided before discussion began.]
```

## Rules

- **Every extraction traces to the transcript or email.** Don't infer decisions that weren't spoken — but do label interpretive reads explicitly so they're distinct from facts.
- **Commitments are the highest-value extraction.** Verbal commitments with no issue are the things that slip.
- **Rationale travels with the decision.** A decision without its why is just a rule nobody understands.
- **Team instincts are as important as formal decisions.** They predict which decisions will hold.
- **Political context requires confirmation before writing to org-dynamics.** Surface first.
- **People files get updated without asking.** They're descriptive, not political.
- **Always close agent-browser session after Roam transcript fetch.** Run `agent-browser --session roam-scan close`.
- **Skip empty meetings.** If no transcript was generated, note it and move on.
- **Propagate outward.** Don't let valuable extractions sit only in meeting-notes.md. Route to canonical files.

## Trigger

`/meeting-notes`, "scan meetings", "what happened in meetings"
