---
name: users
description: Track potential users, manage relationships, surface follow-ups
---

# Users

## Purpose

Track and manage relationships with potential {{PRODUCT}} users across Signal, email, and Telegram. This is relationship management, not feedback collection. The feedback skill tracks bugs and product issues. This skill tracks people: who they are, what they care about, where the conversation lives, and when to follow up.

## Reads
- `docs/users/users.md` — current pipeline state
- `docs/product/master-prd-roadmap.md` — master PRD (ICP definitions, roadmap context)
- `cache/feedback-monday.md` — cross-reference if a pipeline contact reported something

## Writes
- `docs/users/users.md` — pipeline state (source of truth)
- `docs/users/users-log.md` — append-only interaction log

## Dependencies
- **Signal MCP** (`mcp__signal`): Read conversations with tracked contacts
- **Google Workspace MCP** (`mcp__google-workspace`): Search Gmail for conversations with tracked contacts
- **Slack skill** (`.claude/skills/utils/slack.md`): Check if pipeline contacts surface in Slack channels (introductions, mentions)

---

## Users File Schema

`docs/users/users.md` maintains one section per contact:

```markdown
## Users

*Updated: YYYY-MM-DD*
*Total contacts: X | Active: X | Cold: X | Converted: X*

### [Name]

| Field | Value |
|-------|-------|
| Channel | Signal / Email / Telegram / Multiple |
| Source | {{LAUNCH_EVENT}} booth / Intro from [person] / Inbound / etc. |
| Added | YYYY-MM-DD |
| Last Contact | YYYY-MM-DD |
| Temperature | Hot / Warm / Cold |
| Stage | New / Talking / Evaluating / Onboarding / Converted / Stalled |
| What they care about | [1-2 sentences in their words] |
| Blocker | [What's stopping them from using {{PRODUCT}}, if known] |
| Next action | [Specific next step, owned by {{YOUR_NAME}}] |
| Next action by | YYYY-MM-DD |
| Notes | [Context, links, background] |

---
```

### Temperature Rules
- **Hot**: Active conversation in last 3 days, expressed clear intent
- **Warm**: Conversation in last 7 days, interested but no commitment
- **Cold**: No contact in 7+ days, or conversation stalled

### Stage Definitions
- **New**: Just met, no real conversation yet
- **Talking**: Active back-and-forth, learning about each other
- **Evaluating**: They're considering {{PRODUCT}}, asking specific questions
- **Onboarding**: They've agreed to try it, helping them get set up
- **Converted**: They're using {{PRODUCT}} with real funds
- **Stalled**: Conversation died, needs re-engagement or drop

---

## Interaction Log

`docs/users/users-log.md` is append-only. Every scan appends new interactions found:

```markdown
---

## YYYY-MM-DD

### [Name] — [Channel]
- **Summary**: [1-2 sentences of what was discussed]
- **Their signal**: [What they said that matters — interest, objection, question]
- **Next step**: [What {{YOUR_NAME}} should do next]
```

---

## Process

### Adding Contacts

When {{YOUR_NAME}} says "add [name]" or describes meeting someone:

1. Create their section in `users.md` with all known fields
2. Search Signal, Gmail for any existing conversation history
3. Log the initial interaction in `users-log.md`
4. Set temperature and stage based on context

### Scanning for Updates

When invoked as part of daily or standalone:

1. **Read current users** — load `docs/users/users.md`
2. **For each active contact (not Stalled/Converted):**
   - If channel is Signal: `mcp__signal__signal_search_chat` or `signal_get_chat_messages` for their name or chat
   - If channel is Email: `mcp__google-workspace__search_gmail_messages` for their name/email
   - If channel is Telegram: note as "manual check needed" (no MCP yet)
3. **Compare last contact date** against today:
   - If new messages found: update Last Contact, log interaction, reassess temperature
   - If no new messages and Last Contact > 7 days: flag as going cold
4. **Update users.md** with any changes
5. **Append new interactions to users-log.md**

### User Review

When {{YOUR_NAME}} asks "user review" or "who needs follow-up":

1. Read users file
2. Sort by urgency:
   - **Overdue**: Next action date has passed
   - **Going cold**: Warm contacts with no interaction in 5+ days
   - **Needs response**: They messaged, {{YOUR_NAME}} hasn't replied
   - **On track**: Active conversation, next action upcoming
3. Output the review (see format below)

---

## Output Format

### When invoked standalone

```markdown
## Users — [Date]

**Summary:** X active contacts, X need follow-up, X going cold

### Needs Action Now
| Contact | Channel | Last Contact | Issue |
|---------|---------|--------------|-------|
| [Name] | Signal | Feb 18 | They asked about {{STABLECOIN_A}}, no reply |
| [Name] | Email | Feb 15 | Going cold, was interested in staking |

### Active Conversations
| Contact | Stage | Temperature | Next Action | By |
|---------|-------|-------------|-------------|----|
| [Name] | Evaluating | Hot | Send them TestFlight link | Feb 21 |

### New Interactions Found
- **[Name]** ([Channel]): [Summary of what they said]

### Stalled (consider re-engaging or dropping)
- [Name] — last contact [date] — [reason it stalled]
```

### When invoked by daily skill

Return only:
- Contacts needing follow-up today (overdue next actions, going cold)
- New messages from tracked contacts found in scan
- These feed into the Kill List as action items

---

## Integration with Daily

The daily skill should invoke the users scan as part of Step 1 (Ensure Fresh Data). User follow-ups appear in the Kill List when:
- A next action date is today or overdue
- A contact is going cold (Warm, no interaction in 5+ days)
- A contact sent a message that needs a reply

In the daily output, user follow-ups appear under the **Pipeline Follow-ups** section or integrated into the Kill List with context.

---

## Rules

### Accuracy
- **Only log interactions that actually happened.** Don't infer conversations.
- **Use their words for "what they care about."** Don't interpret.
- **Every temperature/stage change must trace to a real interaction.**

### Privacy
- **Signal messages stay in users-log.md only.** Don't copy sensitive content to other files.
- **Don't include wallet addresses or financial details** in pipeline notes unless the contact shared them openly.
- **User files are local only.** Don't sync to Google Docs unless explicitly asked.

### Hygiene
- **Review stalled contacts monthly.** If no re-engagement plan, move to a "Dropped" section.
- **Converted contacts stay in users file** but stop getting scanned. They graduate to the feedback skill if they report issues.
- **Don't let the pipeline grow past 30 active contacts** without explicit conversation about capacity.

---

## Trigger

"users", "user pipeline", "who needs follow-up", "user review", "add [name]", "check on [name]"
