---
name: feedback
description: Collect and prioritize feedback
---


# Feedback Engine

## Purpose
Collect, tabulate, and prioritize user feedback from all sources into a Monday board. One deduplicated issue per item, with mention counts and all sources listed.

## Reads
- `docs/feedback/` — prior feedback
- `docs/product/master-prd-roadmap.md` — master PRD (feature context, ICP, roadmap)

## Writes
- `docs/users/` — processed feedback
- Monday Feedback board: {{MONDAY_BOARD_URL}}

## Dependencies
- **Slack skill** (`.claude/skills/utils/slack.md`): Run the slack skill first (feedback mode) to pull fresh signals from Slack channels before processing.
- **Signal MCP** (`mcp__signal`): Fetch messages directly from Signal group chats.
- **Google Workspace MCP** (`mcp__google-workspace`): Read input sheets, write to master sheet.

---

## Input Sources

### 1. Slack scan
Run the slack skill (feedback mode) first. It returns user/tester complaints, feature requests, bug reports from non-engineering channels, and sentiment signals.

### 2. Signal scan
Fetch messages from these Signal group chats using Signal MCP tools:

| Chat | What to look for |
|------|-----------------|
| {{PRODUCT}}-Prod-Testing | Bug reports, UX feedback, feature requests, strategic debate, competitive intel |
| ANF/PRO {{LAUNCH_EVENT}} | Event feedback, user reactions at booth, partnership interest, live bugs |
| Product-ANF-{{COMPANY}} | Product decisions, cross-team feedback |
| AP / {{YOUR_NAME}} | Direct user feedback from Alex Pruden (former {{CHAIN}} CEO) |

**How to scan:**
1. `mcp__signal__signal_list_chats` — list chats if you need to discover new relevant groups
2. `mcp__signal__signal_get_chat_messages` — pull recent messages (use `limit` param, start with 100)
3. `mcp__signal__signal_search_chat` — search for specific keywords (e.g. "bug", "broken", "feedback", "swap")

**What to extract:**
- Bug reports and UX friction (from testers and external users)
- Feature requests and suggestions
- Strategic signals ({{YOUR_NAME}}'s positions on product direction, competitive intel)
- Positive signals (praise, excitement, partnership interest)
- Attribute every item: who said it, which chat, date

**What to skip:**
- Logistics (shift schedules, travel, food)
- Banter and off-topic conversation (Crypto Minds is mostly social, skip unless crypto/privacy strategy content)
- Messages that are just reactions or acknowledgements

### 3. Google Sheets (input sources to merge from)
- **Feedback Log (F&F + internal):** {{GOOGLE_SHEETS_URL}}
  - Sheet "Feedback": Source, Reporter, Feedback, Type, Severity, Status, Notes
  - Sheet "Survey 2": Tally-style survey responses (Timestamp, use case, ratings, bugs, device, recommendations)
- **{{LAUNCH_EVENT}} Booth Form (Responses):** {{GOOGLE_SHEETS_URL}}
  - Sheet "Form Responses 1": Timestamp, Name, Day, observations, excitement (1-5), understanding (1-5), confusion (1-5), UI clarity (1-5), {{CHAIN}} knowledge (1-5), overall feedback, crypto knowledge level, unanswered questions, notes

### 4. {{FORM_TOOL}}
Feedback forms (use Tally MCP server when available). Key forms:
- {{LAUNCH_EVENT}} field feedback: https://{{FORM_TOOL}}/r/b5Wz8L
- {{LAUNCH_EVENT}} booth feedback: https://forms.gle/TZS2W1gsCjxLyDWS6

### 5. Other
- **App Stores**: iOS App Store, Google Play reviews
- **Analytics**: Amplitude/Mixpanel user behavior signals
- **Notes**: Apple Notes, Obsidian inbox, Google Docs, random jots

---

## Master Board (Monday)

**URL:** {{MONDAY_BOARD_URL}}
**Board ID:** `18402201375`

One deduplicated issue per item. Use `mcp__claude_ai_monday_com` to read and write.

**Expected columns (verify with `get_board_info`):**

| Column | Description |
|--------|-------------|
| ID | Auto-assigned by Monday |
| Issue | The feedback, in the reporter's language. No interpretation. |
| Category | Bug, UX Friction, Feature Request, Positive Signal, Strategic |
| Severity | Critical, High, Medium, Low (leave blank for Positive Signals) |
| Mentions | Count of times reported across all sources |
| Sources | All sources that mentioned it |
| Reporters | Names of people who reported it |
| First Reported | Date first seen |
| Status | Open, In Progress, Fixed, Won't Fix |
| Platform | Extension, Android, iOS, All, N/A |
| Notes | Context, links, or PM commentary |

**Note:** Board column structure may differ from above. Always read the board schema first with `get_board_info` or `get_full_board_data` before writing.

---

## Process

### Step 1: Gather fresh feedback
Run these in parallel:
1. Run the slack skill (feedback mode) to get fresh Slack signals
2. Run Signal scan (pull last 100 messages from each chat listed above)
3. Read Monday feedback board via `mcp__claude_ai_monday_com__get_board_items_page` (board 18402201375) to check for new items since last scan
4. Check {{FORM_TOOL}} if MCP is available

### Step 2: Read existing master board
Use `mcp__claude_ai_monday_com__get_board_items_page` with board ID `18402201375` to get all existing items.
Find the last item to know what already exists.

### Step 3: Deduplicate
- Compare new feedback against existing items in the master board
- If a new item matches an existing row: increment Mentions count, add the new source to Sources column, add reporter name
- If a new item is genuinely new: create a new row with the next ID

**Deduplication rules:**
- Same issue from multiple sources = one row, increment Mentions, list all sources
- Match on meaning, not exact wording (e.g. "Hardware wallet support" = "Ledger integration" = "Yubikey support")
- When in doubt, keep separate rows rather than wrongly merging

### Step 4: Write to master board
Use `mcp__claude_ai_monday_com__create_item` for new items and `mcp__claude_ai_monday_com__change_item_column_values` for updates (incrementing Mentions, adding Sources).
Board ID: `18402201375`.

### Step 5: Sync local markdown
Update `cache/feedback-monday.md` to reflect the current state of the master board.

---

## Anti-Hallucination Rules

These are mandatory. Never skip them.

1. **Every row must trace to a specific source and reporter.** No anonymous feedback. If you can't attribute it, don't add it.
2. **Use the reporter's language, not your interpretation.** "This shit is fast" not "Users praised transaction speed."
3. **If feedback is ambiguous, add the verbatim quote in Notes.** Let the reader decide.
4. **Never infer severity.** Only mark severity if:
   - It was explicitly assessed in the source, OR
   - It's obviously a blocker (data loss, crash, funds lost, can't complete core flow)
5. **Positive Signal rows preserve exact quotes.** Don't summarize excitement.
6. **Don't create feedback items from your own analysis.** Only capture what someone actually said or reported.

---

## Output Format (for markdown sync)

### Summary
[2-3 sentence executive summary of feedback themes]

### Critical Issues (Blockers)
| Issue | Source | Frequency | Status |
|-------|--------|-----------|--------|

### High Priority (Friction)
| Issue | Source | Frequency | Status |
|-------|--------|-----------|--------|

### Feature Requests
| Request | Source | Frequency | Priority |
|---------|--------|-----------|----------|

### Positive Signals
| Signal | Source | Date |
|--------|--------|------|

---

## Deep Analysis Mode

**Trigger:** "analyze feedback", "what are customers really saying", "find unmet needs"

Goes beyond counting and categorizing. Looks for the problems behind the feature requests.

### Process
1. **Cluster into themes.** Name each theme with a direct customer quote, not a category label. "I don't know if it went through" not "Transaction Status Visibility."
2. **For each theme, answer:**
   - How many customers mentioned it?
   - What's the underlying job-to-be-done?
   - What are they using as a workaround right now?
3. **Identify the "screaming in the data" insight.** What's the pattern everyone's missing? The thing that connects multiple themes into one deeper problem.

### Rules
- **Always attribute who reported it.** Every feedback item must include the person's name and channel.
- Ignore feature requests. Focus on problems.
- Use customer language, not product language.
- If feedback says "add X feature," ask what pain X would solve. That's the real signal.

### Output Format

**Themes** (ordered by frequency)

| Theme (in their words) | Mentions | Job-to-be-done | Current workaround |
|------------------------|----------|----------------|--------------------|
| "[Customer quote]" | X | [What they're trying to accomplish] | [What they do today instead] |

**The insight everyone's missing:**
[1-2 sentences. The pattern that connects multiple themes into one deeper unmet need.]

**Implications for roadmap:**
- [What this means for what we build next]

---

## Trigger
"run feedback-engine" or "collect feedback" or "analyze feedback" or "what are customers really saying" or "find unmet needs"
