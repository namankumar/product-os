---
name: narrative
description: Create positioning and narrative docs
---

# Narrative Workshop

## Purpose
Discuss, stress-test, and craft product narratives that drive adoption. Narratives are the reason people care — they're the story the market tells itself about your product. Especially in crypto, where attention is the scarcest resource and conviction spreads through story before it spreads through usage.

This skill treats narratives as strategic assets, not marketing copy.

## Dependencies
- `.claude/skills/utils/voice.md` — apply before all output
- `.claude/skills/utils/writeDocs.md` — apply when editing existing docs

## Reads
- `context/room/<group-name>.md` — room analysis from read-room skill (audience beliefs, what resonates, influence patterns)
- `docs/strategy/` — strategic bets
- `docs/market-research/` — landscape, market data
- `docs/narrative/` — prior narratives

## Writes
- `docs/narrative/` — narratives, positioning docs

## Trigger
"run narrative" or "create a narrative" or "workshop narratives" or "test a narrative"

## Why This Matters
Products don't win on features. They win on the story people repeat to each other. In crypto this is amplified — communities form around narratives before products ship. The right narrative:
- Creates a category (not just a product)
- Gives advocates language to recruit others
- Survives the telephone game — still works after 3 retellings
- Aligns internal teams on what matters and what doesn't

## Process

### Step 1: Gather Context
Before creating anything, ask the user the following questions. Do not proceed until these are answered.

1. **What product or initiative is this narrative for?**
2. **Who is the primary audience?** (users, investors, developers, community, press — pick one primary, then load relevant room files from `context/room/<group-name>.md` for their current positions and what moves them)
3. **What do they currently believe?** (the existing mental model — room files track position evolution and what shifted people before)
4. **What do you want them to believe after hearing this?** (the shift)
5. **What is true today that makes this narrative credible?** (proof points, traction, technical facts)
6. **What is the competitive narrative?** (what are rivals saying, what story does the market default to)
7. **What emotional register?** (conviction, urgency, inevitability, rebellion, trust, simplicity)
8. **Where will this narrative live?** (landing page, Twitter/X, pitch deck, docs, community calls, all of the above)

### Step 2: Map & Quantify Current Narratives
Before drafting new narratives, analyze the existing narrative landscape. Use the deep-researcher agent to systematically gather data.

**For each major narrative currently active in the market:**

1. **Identify the narrative**
   - Core claim (one sentence)
   - Who's pushing it (protocols, influencers, VCs, media)
   - When did it emerge (timeline: origin → amplification → saturation)

2. **Quantify percolation** (how widely spread it is)
   - **Twitter/X metrics:**
     - Mention volume (tweets/week mentioning the narrative)
     - Influencer adoption (which KOLs are repeating it, follower count, engagement)
     - Search interest (Twitter search volume trends)
   - **Google Trends:** Search volume over time for key phrases
   - **Media coverage:** Number of articles, tier of publications (Tier 1: CoinDesk, The Block vs Tier 3: blogs)
   - **Protocol adoption:** How many projects have built narratives around this theme
   - **VC/thought leader mentions:** Conference talks, podcasts, investment theses

3. **Assess narrative lifecycle stage**
   - **Emerging** (10-20% awareness, early adopters only)
   - **Growing** (20-50% awareness, crossing into mainstream)
   - **Saturated** (50%+ awareness, everyone's talking about it)
   - **Declining** (past peak, losing mindshare)

4. **Analyze how your proposed narrative will mesh**
   - **Complement:** Fits naturally with existing narrative (e.g., "privacy" + "regulatory compliance" = "compliant privacy")
   - **Compete:** Direct challenge to existing narrative (requires evidence your framing is superior)
   - **Reframe:** Takes existing narrative and shifts the angle (e.g., "decentralization" → "user sovereignty")
   - **Ride:** Piggybacks on saturated narrative to gain initial attention, then differentiates
   - **Ignore:** Orthogonal to existing narratives, creates new category

**Output:** Create a Current Narrative Analysis table and include in Section 1 of the doc:

| Narrative | Core Claim | Percolation Score | Stage | How We Mesh |
|-----------|-----------|------------------|-------|-------------|
| [Narrative 1] | [One sentence] | HIGH/MED/LOW (with metrics) | Saturated | Complement / Compete / Reframe / Ride / Ignore |
| [Narrative 2] | [One sentence] | HIGH/MED/LOW (with metrics) | Growing | [Strategy] |

**Percolation Scoring:**
- **HIGH:** 100K+ weekly mentions, 10+ major influencers, Google Trends >50, Tier 1 media coverage
- **MEDIUM:** 10K-100K weekly mentions, 3-10 influencers, Google Trends 20-50, Tier 2 media
- **LOW:** <10K weekly mentions, <3 influencers, Google Trends <20, niche coverage only

**Critical Analysis:**
- Which narratives are **saturated** (everyone believes this now — hard to differentiate)?
- Which narratives are **declining** (entering the trough of disillusionment)?
- Which narratives are **emerging** (early enough to own, but risky)?
- What **narrative gap** exists (a story the market needs but no one's telling)?

### Step 3: Draft Candidate Narratives
Generate 2-3 candidate narratives. Each candidate gets:

- **Name** — a short handle (e.g., "The Privacy Default", "Compliant Freedom")
- **Core claim** — one sentence. If someone remembers nothing else, this is it.
- **Supporting logic** — 2-3 sentences that make the claim feel inevitable, not aspirational
- **Proof points** — 2-3 concrete facts, numbers, or comparisons that anchor credibility
- **One-liner version** — tweet-length. The version that survives the telephone game.
- **Enemy** — what this narrative positions against (a competitor, a status quo, a belief)
- **Emotional hook** — the feeling it creates (not the feature it describes)

### Step 4: Stress-Test Each Narrative
Run every candidate through these five tests. Present results as a scorecard and discuss with the user.

| Test                  | Question                                                                  | Pass Criteria                                          |
| --------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------ |
| **Telephone Test**    | Can someone repeat this after hearing it once?                            | Core claim survives 3 retellings without distortion    |
| **Belief Shift Test** | Does this change what the audience believes, or just inform them?         | Clear before/after in audience mental model            |
| **Enemy Test**        | Is there a clear "old way" this narrative defeats?                        | Named enemy — a competitor, a default, a misconception |
| **Proof Test**        | Is there something true today that makes this credible, not aspirational? | At least 2 verifiable proof points that exist now      |
| **Tribe Test**        | Would someone put this in their bio / repeat it to signal identity?       | Creates in-group language, not just understanding      |

**Scoring:**
- Each test: **Pass / Partial / Fail**
- A narrative that fails 2+ tests needs rework or should be cut.
- A narrative that passes all 5 is rare — flag it as the lead candidate.

### Step 5: Evaluate Strategic Fit
Score the lead narrative(s) against these criteria before finalizing.

| Criteria | Question | Signal |
|----------|----------|--------|
| **Market Size** | Does this narrative speak to a large and growing audience? | Addressable community size, cultural moment, trend alignment |
| **Defensibility** | Can a competitor co-opt this story? | Unique proof points, first-mover on the framing, technical moat backing the claim |
| **Strengths Alignment (SWOT)** | Does this narrative play to what we can actually deliver? | Team capability, product reality, brand credibility |
| **Precedent** | Has a similar narrative driven adoption elsewhere? | Analogous products/movements that used this framing successfully |

- Rate each **High / Medium / Low**.
- A narrative scoring Low on Strengths Alignment is dangerous — it creates expectations you can't meet.
- Precedent is a confidence signal. No precedent = higher risk, which is fine if the other scores are strong.

### Step 5B: Choose Emotional Register and Funnel Architecture

Most narratives can be delivered in multiple emotional registers. The two primary registers:

- **Fear:** "You're at risk. Act now." — Creates urgency, spikes action, short shelf life. Best for launch hooks and awareness.
- **Empowerment:** "You deserve better. Upgrade." — Creates identity, sustains growth, broader appeal. Best for long-term positioning and retention.

For each lead narrative, draft messaging in both registers. Then decide sequencing:

```
LAUNCH (Week 1-2):    Fear register — spikes downloads / signups
TRANSITION (Week 2-4): Empowerment register — sustains growth, broadens audience
LONG-TERM (Month 2+):  Durable positioning — the identity people stay for
```

Fear gets them in the door. Empowerment is why they stay and tell others. Nobody wants to join a community built on "we're all scared." They want "we're all smart enough to have figured this out."

**Funnel architecture:** Multiple narratives can coexist if they serve different funnel stages rather than competing for one headline.

| Funnel Stage | Role | Example |
|-------------|------|---------|
| **Awareness** | Hook — creates urgency or curiosity, drives click | Fear-register lead narrative |
| **Consideration** | Positioning — describes the product clearly for any visitor | Empowerment-register lead narrative |
| **Trust** | Credibility signal — removes "is this legit?" objection | Institutional proof points, partnerships |
| **Conversion** | Channel-specific — meets the user in their existing workflow | Integration-specific messaging |
| **Retention** | Identity — the long-term brand the user identifies with | Durable positioning |

Map each candidate narrative to a funnel stage. If two narratives compete for the same stage, one must be cut or demoted to supporting.

**"Container" narratives must be filled.** If a narrative is abstract (e.g., "replace failing OpSec"), ask: for what, specifically? Identify the concrete threats or benefits underneath, build a taxonomy, then segment messaging by audience — each audience gets the version of the narrative that matches their specific pain or aspiration.

### Step 6: Create the Narrative Doc
Generate a markdown file in `docs/` using the structure below.

### Step 7: Review
Walk the user through the doc. Pressure-test the lead narrative together. Ask what needs to change before this goes to the team or market.

## Document Structure

```
# [Product] — Narrative Playbook

**Owner:** Product — [Name]
**Date:** [Month Year]
**Status:** Draft
**Audience:** [Primary audience for these narratives]

---

**TL;DR:** [One sentence — the narrative we're betting on and why.]

---

## 1. Narrative Landscape
## 2. Lead Narrative
## 3. Supporting Narratives
## 4. Narrative Scorecard
## 5. Emotional Register & Funnel Architecture
## 6. Activation Plan
## 7. Anti-Narratives
## 8. Open Questions

---

## Version History
| Date | Change |
|------|--------|
```

## Section-Specific Guidance

**1. Narrative Landscape** — What stories currently dominate the market with quantified percolation analysis. Include the Current Narrative Analysis table from Step 2 showing each major narrative's core claim, percolation score (with metrics: Twitter mentions/week, influencer count, Google Trends score, media tier), lifecycle stage (emerging/growing/saturated/declining), and how your proposed narrative will mesh with it (complement/compete/reframe/ride/ignore). Follow with narrative gap analysis: what story is missing or wrong in the current landscape. Conclude with "why now" for your narrative — the timing rationale based on where existing narratives are in their lifecycle.

**2. Lead Narrative** — The primary narrative you're recommending. Full detail: core claim, supporting logic, proof points, one-liner, enemy, emotional hook. Include the stress-test scorecard inline. Bold the core claim — it's the most important sentence in the doc.

**3. Supporting Narratives** — 1-2 secondary narratives that reinforce the lead. These might target different audiences or different channels. Each gets the same structure as the lead but briefer. Note how they ladder up to the lead narrative — they should amplify, not compete.

**4. Narrative Scorecard** — Summary table of all candidates (including any that were cut) with their stress-test and strategic-fit scores. Makes the selection rationale transparent.

**5. Emotional Register & Funnel Architecture** — Show each lead/supporting narrative in both fear and empowerment registers. Map narratives to funnel stages (awareness, consideration, trust, conversion, retention). Include the register transition timeline: what register leads at launch, when to transition, what the long-term identity voice sounds like. If a narrative is a "container" (abstract framing), include the filled taxonomy of specific threats/benefits underneath with audience-segmented messaging.

**6. Activation Plan** — Where and how each narrative gets deployed. Table format:

| Narrative | Channel | Format | Owner | Timing |
|-----------|---------|--------|-------|--------|

Channels include: landing page, Twitter/X, pitch deck, community calls, docs, press, partnerships. Every narrative needs at least one channel where it's the primary message — not a background theme.

**7. Anti-Narratives** — The 2-3 stories your competitors or critics will tell against you. For each: what they'll say, why it's wrong (or partially right), and how your narrative pre-empts it. If you can't answer an anti-narrative, your lead narrative has a hole.

**8. Open Questions** — Unresolved narrative decisions. Each with context and a recommendation. Examples: "Should we lead with privacy or compliance?" "Do we name the competitor or stay above it?"

## Formatting Rules

- **Bold the core claim everywhere it appears.** It should be impossible to miss.
- **One-liners in blockquotes.** These are the quotable versions.
- **No jargon without translation.** If the audience wouldn't use the word, neither should the narrative.
- **Tables: 5 columns max.**
- **Bullets: 2 lines max each.**
- **No emojis.** Unless explicitly requested.
- **Enemy should be named, not vague.** "Legacy wallets" not "the old way." "Custodial exchanges" not "centralized solutions."
- **Positioning needs a verb. Claims don't count.** "Most private wallet in crypto" is a superlative on a noun — it invites debate and isn't falsifiable. "Privately hold mainstream crypto assets" is a verb — it's concrete and verifiable. Superlative claims ("most," "best," "first") work as credibility badges underneath a real positioning, never as the headline.
- **Fear-register messaging in blockquotes, empowerment-register messaging as tagline candidates.** Label which register each message uses.

## Crypto-Specific Guidance

Narratives in crypto carry extra weight because:
- **Communities form around narratives before products.** The story is the product's first distribution channel.
- **CT (Crypto Twitter/X) is the proving ground.** If a narrative can't survive quote-tweet culture, it won't spread.
- **Narratives cycle.** Privacy, decentralization, compliance, UX — each has seasons. Know where you are in the cycle and whether you're riding a wave or creating one.
- **Credibility is fragile.** One overpromise and the narrative flips from bullish to "vaporware." Every claim must be anchored to something real.
- **Memes > whitepapers for initial distribution.** The one-liner matters more than the supporting logic for first contact. The logic matters for retention.

## Tools for Quantifying Narrative Percolation

When analyzing current narratives in Step 2, use these tools to gather quantitative data:

**Twitter/X Analysis:**
- Twitter Advanced Search for phrase/hashtag volume over time
- Social listening tools (Brandwatch, Sprout Social) for mention tracking
- Manual influencer audit: search key phrases, note who's tweeting, follower counts, engagement rates

**Search Trends:**
- Google Trends for search interest over time (compare multiple narratives)
- Set timeframe to 12 months minimum to see lifecycle stage

**Media Coverage:**
- CoinDesk, The Block, Decrypt (Tier 1)
- CryptoSlate, Cointelegraph (Tier 2)
- Blogs, Substack (Tier 3)
- Count articles, note publication dates to map amplification timeline

**Protocol/Project Adoption:**
- Scan landing pages, pitch decks, Twitter bios for narrative keywords
- Note how many protocols are positioning around the same narrative

**VC/Thought Leader Signals:**
- Messari research reports, {{INVESTOR_FIRM}} crypto blog, Pantera newsletters
- Conference talks (ETH Denver, Consensus, Devcon) — what themes dominated panels?

**Deep Research Agent:**
- Use for systematic multi-source narrative mapping when analyzing 3+ narratives
- Consolidates Twitter, Google Trends, media, and protocol data into percolation scores

## Output Destination
- Save as `.md` in `docs/`
- Never write directly to Google Drive/Docs — local markdown first, output to Drive only when explicitly requested
