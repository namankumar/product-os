---
name: reply
description: Craft replies in {{YOUR_NAME}}s voice for any channel
---

# Reply

## Purpose
Craft replies for any channel (Signal, Slack, email, Telegram) that advance {{YOUR_NAME}}'s strategic position. Every reply should feel calm, factual, and non-confrontational while steering the conversation toward {{YOUR_NAME}}'s goals. Channel-agnostic: the process is the same whether replying in a group chat, a DM, or an email thread.

## Reads
- `context/org-dynamics.md` — political context ({{FOUNDER_CEO}}/{{PARTNER_CEO}} split, Signal group dynamics, role risk)
- `context/current-context.md` — decision-making context (what matters now, how to frame)
- `context/narrative-map.md` — active narratives (replies should reinforce, not contradict)
- `context/room/` — room analysis (who's who, influence, temperature)
- `context/room/people/<name>.md` — person profiles (what persuades them, how they see {{YOUR_NAME}})
- The conversation itself (via Signal MCP, Slack MCP, agent-browser, or provided by {{YOUR_NAME}})

## Writes
- Replies for {{YOUR_NAME}} to send (displayed in conversation, not saved to file)

## Trigger
`/reply`, "how do I reply to this", "help me respond to [person]", "what should I say in [channel]", "draft a reply to [person]"

## Process

### Step 1: Load the Room
Before crafting any reply, load the room analysis for this group from `context/room/<group-name>.md`. If stale or missing, run read-room first.

Understand from the room file:
- Who's active and what they want
- Who carries authority
- Current temperature and open tensions
- {{YOUR_NAME}}'s position in this room

### Step 2: Read Latest Messages
Read the conversation since the room file was last updated. Understand:
- What was said and by whom
- What they're replying to
- Conversational momentum (is consensus forming? is someone pushing a direction?)

### Step 3: Identify the Person
Load their person file from `context/room/people/<name>.md`. This has everything: motivation, what persuades them, communication style, how they see {{YOUR_NAME}}, and how to position {{YOUR_NAME}} as a leader to them. If no person file exists, derive what you can from the room file and flag the gap.

Key questions the person file should answer:
- What is their deep motivation?
- What persuades them? What doesn't work?
- What's their authority in the room?
- What's their communication style?
- How do they see {{YOUR_NAME}}, and how should {{YOUR_NAME}} show up to them?

### Step 4: Build Context From Data + Ask What You Can't Derive

**First, derive what you can from the messages.** Group chats are full of signal if you read carefully:

- **Authority:** Who gets challenged and who doesn't? If someone states a position and nobody pushes back, that's authority. If someone states a position and three people pile on to disagree, that's low standing.
- **Influence patterns:** Who echoes whom? Track who speaks after whom and whether they're adding new thinking or amplifying.
- **Depth vs. surface:** Does this person engage with specifics or generalities? Do they bring external knowledge or only react to what's in front of them?
- **Who responds to whom:** If {{YOUR_NAME}}'s messages get immediate engagement from one person but silence from another, that's relationship data.
- **Deference patterns:** When two people disagree, who backs down? When an authority figure speaks, does debate continue or stop?
- **What changed someone's mind:** Track position shifts and what caused them. That tells you what persuades them.

**Then ask {{YOUR_NAME}} only what you genuinely can't derive from messages:**

- **What's the subtext?** Why are they saying this now? Is there an offline conversation driving this?
- **What do you want them to feel after reading your reply?** Not just what position to take, what experience to create.
- **What can't you say directly?** Sometimes the goal is to communicate something without stating it.
- **Are there names or relationships I should know for this specific reply?** Naming someone by title in a reply can signal escalation or peer-level engagement.

**Don't ask what you can figure out.** If the messages show someone carries the room, don't ask "does this person have influence?" Read the data.

### Step 5: Identify Your Goal
What does this reply need to accomplish? Map to {{YOUR_NAME}}'s strategic goals:
1. Own user relationships
2. Own Zcash migration as independent asset
3. Don't get locked into {{FOUNDER_CEO}} or {{PARTNER_CEO}}'s side
4. Reduce accountability without authority (don't make promises under pressure)
5. Keep information advantage
6. Maintain/strengthen external endorsements of direction

If the reply is purely operational (bug response, timeline question), keep it short and direct. Not every message needs strategic depth.

### Step 6: Respect Their Standing
Before style and technique, get the social dynamics right. These are successful people: founders, CTOs, fund managers, community leaders. The reply should reflect genuine respect for their experience and perspective, not manage them as stakeholders.

- **Acknowledge their expertise as real.** When someone frames a vision from deep industry experience, treat their input as informed perspective, not feedback to be processed.
- **Recognize authority differences.** Not everyone in the room has equal standing. Show it through action, not words. When a senior figure reports a bug, you don't say "we're aware." You take their specific details, forward to the relevant team's leadership by name, and report back that things are moving. That sequence shows: (1) their report went to the top, not into a queue, (2) naming leadership signals peer-level escalation, (3) reporting impact confirms their authority had real effect.
- **Engage as a peer on substance, match standing on operations.** You can redirect someone's strategic framing with your own strategic observation. That's thinking at their level. On operational matters, match the response to their standing.
- **Never condescend by over-explaining.** Build on their framing, don't explain things they already know.
- **When you redirect, redirect with substance they'll respect.** Not "we have other priorities" but a strategic observation at their level.

### Step 7: Write for the Room, Not Just the Person
You're replying to one person but the whole group reads it. Before drafting, map how each active voice would react to both the original message AND your reply:

- **What does each person care about in this thread?** Your reply should address or at least not conflict with what matters to each.
- **Who might push back on your reply?** Anticipate the next 2-3 messages.
- **Who needs to see themselves in your reply?** When you list user segments, each relevant person should see their constituency represented.
- **What concerns does your reply preempt?** A well-crafted reply addresses likely objections without naming them.

The goal: after your reply, the people who matter most should feel their perspective was considered, the people who might push back should have less to push back on, and the direction you want should feel like the room's conclusion.

### Step 8: Identify Their Rhetorical Device
Before matching style, name their primary device. This is separate from tone and register — it's the structural move they're making.

Common devices to look for:
- **Antanaclasis** — same word, different sense ("stands for something / *stand* for something")
- **Anadiplosis** — end of one sentence starts the next ("rejection becomes the brand. the brand becomes...")
- **Antithesis** — opposites in parallel ("not X, but Y" as structure, not corrective)
- **Paradox** — sounds contradictory but isn't
- **Litotes** — understatement through negation ("not nothing")
- **Twisted idiom** — inverted or compressed known phrase
- **Nominalization** — process given a name and turned into actor

Then pick one device for the reply: either mirror theirs (shows you're in the conversation) or pick the one that best fits the response goal (see voice.md device selection table).

Device is chosen here, before drafting. Not discovered after.

### Step 8b: Match Their Style
Match the person's communication style, not {{YOUR_NAME}}'s default. Key dimensions:
- **Sentence structure:** Flowing (commas, no periods between connected thoughts) vs. staccato (short bursts)
- **Tone:** Warm-factual, passionate-direct, analytical-detailed, technical-specific
- **How they assert:** Facts that lead to one conclusion? Positions as identity? Evidence?
- **Punctuation:** Some use periods heavily, some barely use them. Match it.
- **Format:** Multi-message or single block? Short or long?

### Step 9: Apply Persuasion Technique

**Core principle: State facts, not opinions. Let the room draw the conclusion.**

Techniques (use the one that fits):

**Validate then redirect.** When someone pushes a direction you want to reframe:
- Open with genuine acknowledgment
- State a fact that reframes
- Close with where that logic leads
- Never use "but." Use "which means" or just start a new message.

**Ask the question you know the answer to.** When you want the room to arrive at your conclusion:
- Drop a question that has only one honest answer
- Let others respond
- Agree with whoever says what you were thinking

**Plant the fact, go quiet.** When you want to shift direction without leading it:
- Share a data point or observation (link, stat, screenshot)
- Don't editorialize
- Let others react and build on it
- Come back later to endorse the direction that formed

**Elevate someone else's point.** When someone says something that serves your goals:
- Amplify it ("say more about this")
- Build on it with your framing
- Now it's a shared position, not just yours

**Close both sides.** When arguing for a position, cover both user segments / both scenarios:
- "[User type A] needs X because [reason]"
- "[User type B] needs X because [different reason]"
- "So either way, X"

### Step 10: Remove Edges
Before finalizing, scan for:
- **"But"** — replace with "which means," a comma, or a new message
- **"Though" at end of sentence** — sounds dismissive, remove
- **"Who's ready now" / "what we should actually do"** — sounds like correcting someone
- **Direct disagreement** — reframe as building on their point
- **"I think" / "I believe"** — state it as fact or don't say it
- **Questions that sound rhetorical** — genuine questions invite conversation, rhetorical questions challenge
- **Periods between connected thoughts** — for flowing styles, use commas

### Step 11: Commitment Check
Before sending, check: does this reply create an accountability trap?
- **Avoid:** "I'll push this next week," "coming soon," specific timelines you're not confident about
- **Instead:** "Fixing this, will update when it's live" or "good flag, let me look at what's involved"
- **Exception:** When committing fast builds trust (e.g., responding to a senior figure's bug with immediate escalation to named engineers)

## Multi-Message Format
For strategic replies (not bug responses), consider multiple messages:
1. First message: Set the macro frame or acknowledge their point
2. Second message: Deliver the key fact or reframe
3. Third message (optional): Close with the actionable conclusion

Each message should flow from the last. The reader should feel like they're following one thought that builds, not reading three separate arguments.

## Channel-Specific Notes

**Signal groups:** Real-time, informal. Multi-message format works naturally. Match the chat energy.

**Slack:** More structured. Threads vs. channel matters. In-thread can be more detailed. Channel-level should be concise.

**Email:** Formal enough for structure. Can be longer. But the same principles apply: validate, redirect with facts, remove edges, write for all recipients.

## What This Skill Does NOT Do
- Write messages pretending to be someone else
- Make strategic decisions. It crafts replies that advance positions {{YOUR_NAME}} has already chosen.
- Replace reading the room. Always load the room file before replying.

## Example: The Eddy Reply (Feb 21)

**Context:** {{INVESTOR_NAME}} (partner at {{INVESTOR_FIRM}}) said "I like this frame but I'm hoping people come to aleo because they want the full payments + defi + holding any tokens (including RWAs) + value ALEO in its own right!"

**Goal:** Redirect from {{INVESTOR_NAME}}'s aspirational ordering (payments first, holding third) to {{YOUR_NAME}}'s thesis (privacy/holding is the near-term edge) without disagreeing.

**Process:**
1. Validated ("love the full vision!")
2. Deflated the uniqueness of the endgame ("every L1 from Solana to Sui has the same endgame")
3. Connected to time ("it'll take a couple years to play out for {{CHAIN}}")
4. Established the real edge ("privacy narrative is the hottest it's been in years, wallet is the only app live on {{CHAIN}}")
5. Addressed {{INVESTOR_NAME}}'s ALEO token interest ("both for usage and ALEO")
6. Closed with who converts now ("zcash community and commodity traders")
7. Removed all edges: no "but," no periods between flowing thoughts, no rhetorical questions, no "I think"

**Result (three messages, matching {{INVESTOR_NAME}}'s flowing style):**

"love the full vision! absolutely where we're headed long term, every general purpose L1 from Solana to Sui has the same endgame which means it'll take a couple of years to play out for {{CHAIN}}

the privacy narrative is the hottest it's been in years and the wallet is the only app live on {{CHAIN}}, that's what gives ALEO exposure and market leverage in the immediate 6mo

b2b sales cycles take months to years, closest to converting now are zcash community and (potentially) commodity traders sending to each other"
