---
name: competitive-analysis
description: Deep dive on competitors and protocols, extract learnings
---

# Protocol/Business Analysis

## Purpose
Research existing protocols/businesses to extract learnings for your own strategy. Use this when you need to understand how competitors succeeded, validate market precedent, or reverse-engineer successful GTM strategies.

## Reads
- `docs/market-research/` — prior analyses, market context

## Writes
- `docs/market-research/` — new analyses
- `docs/market-research/` — market data

## Trigger
"analyze [protocol/company]" or "research how [X] succeeded" or "understand [competitor]'s strategy" or "run competitive-analysis"

## Process

### Step 1: Gather Context
Ask the user:
1. **What protocol/business are we analyzing?** (name)
2. **What's the purpose?** (inform our strategy, validate market exists, understand GTM playbook)
3. **What specific questions do we need answered?** (origin story, user acquisition, what made them win)
4. **Are there specific claims to fact-check?** ("Did Arthur Hayes' tweet cause the rally?")

### Step 2: Run Research Questions
Use the framework below to systematically research the protocol. Not all sections are always needed — focus on what's relevant to the purpose.

### Step 3: Validate the Numbers (Critical Analytics)
Before trusting any self-reported metrics, cross-reference against on-chain data. This is non-negotiable.

1. **Find the Dune dashboard** — search `dune.com/[protocol-name]` and related dashboards. This is the authoritative source.
2. **Cross-reference:** Self-reported volume/revenue vs Dune vs DeFiLlama vs Token Terminal. Note discrepancies.
3. **Run unit economics:** Fee rate x volume = expected revenue. Does it match tracked revenue? If not, why?
4. **Assess growth sustainability:** Is growth organic or incentive-driven? What's the trend — accelerating, flat, or declining?
5. **Flag red flags:** What doesn't add up? Closed source? Unnamed partners? Team size vs revenue?
6. **Produce honest verdict:** What's real and what's smoke? Present to the user before proceeding.

### Step 4: Create Analysis Doc
Generate markdown file in `docs/` with findings structured per template below. **Include the Table of Contents** after the Key Takeaways section for easy navigation.

### Step 5: Extract Recommendations
End with "What We Can Steal vs What to Avoid" section tailored to user's context, plus explicit overlap mapping and growth levers unique to us.

---

## Research Questions by Category

### **1. ORIGIN STORY & TRUST BUILDING**

**Goal:** Understand credibility signals and trust timeline

**Questions:**
1. **What's [Protocol]'s origin story?**
   - When founded, by whom? Background of founders?
   - Initial product vs pivot (if any)?
   - Near-death experiences / survival moments that proved resilience?
   - What almost killed them and how did they survive?

2. **How did it gain trust?**
   - Audits (which firms, when, cost)?
   - Battle-testing (TVL milestones, how long at each level)?
   - What credibility signals did users respond to?
   - What competitors got hacked/failed while they survived?

3. **Timeline: How long did trust-building take?**
   - Launch → medium trust (users with $100K+ positions): X months
   - Medium trust → high trust (users with $1M+ positions): X months
   - High trust → institutional trust (funds, DAOs, regulated entities): X months
   - Can you skip phases or must you wait?

4. **Why did it win vs competitors?**
   - What was unique/differentiated (that competitors couldn't copy)?
   - What moats were created (network effects, switching costs, regulatory barriers)?
   - What did competitors have that it lacked (and how did it overcome)?

5. **Why did it win? (The one thing)**
   - Strip away all the execution details. What is the single core reason this protocol has adoption?
   - Usually it's a positioning insight, not a feature. (e.g., Houdini: "only compliant privacy option — users aren't choosing Houdini over competitors, they're choosing it over doing nothing")
   - What empty quadrant did they occupy? What market gap existed that nobody else filled?
   - What psychological barrier did they remove that let users act?
   - Does the on-chain data confirm this thesis? (e.g., if they claim privacy is the value, is privacy usage >50% of volume?)
   - **This should be one paragraph.** If you can't say it in one paragraph, you haven't found it yet.

   **Important: "Winning" = market pull and usage, not team/org outcomes.** A protocol can have real adoption and product-market fit while the team runs out of money, quits, or gets acquired. Those are org/funding failures, not product failures. Separate these clearly:
   - **Product win:** Users showed up, volume/TVL grew, behavior was real and recurring
   - **Org failure:** Team couldn't monetize, ran out of runway, governance collapsed
   - A product can win and the org can still fail. (e.g., Zashi: real shielded pool usage, $4-5B market cap coin, Oct 2025 privacy rally — the product had pull. The team quitting was a funding/org failure, not evidence the product didn't work.)

**Output Section:** Origin Story & Trust Timeline

---

### **2. GTM STRATEGY BY USER SEGMENT**

**Goal:** Understand acquisition strategies for different customer types

**A. Retail Users (Small individual users)**

**Questions:**
1. How did [Protocol] acquire retail users?
   - Inbound/organic vs outbound/paid marketing?
   - What was the marketing mix (% breakdown: organic, community, paid)?
   - CAC (customer acquisition cost) per user?
   - Timeline: How long to reach first 10K users? 100K users?
   - **Data sources:** Token Terminal (user growth charts), Dune Analytics (new user cohorts), DeFiLlama (TVL/user timelines)

2. What channels worked?
   - Liquidity mining / token incentives (how much $ in tokens)?
   - Community building (Discord size, governance participation)?
   - Content marketing (SEO rankings, blog traffic)?
   - Integrations (got listed on which aggregators/wallets)?
   - Influencers / thought leadership (who, when, impact)?

3. What was the budget?
   - Token emissions ($ value)?
   - Cash marketing spend?
   - Team size (community managers, marketing leads)?

**B. Whales ($1M+ positions)**

**Questions:**
1. How did [Protocol] acquire whales?
   - Organic (whales found it because product was better)?
   - Outbound BD (evidence of direct sales team, conference networking)?
   - What made whales switch from competitors?
   - Timeline: When did first whales arrive (month X after launch)?

2. What did whales need?
   - Liquidity depth (what $ amount was "enough")?
   - Unique features (what could they ONLY do here)?
   - White-glove support (dedicated channels, priority service)?
   - Governance power (token voting rights, parameter control)?

3. Evidence of whale-focused strategy?
   - Job postings for BD roles?
   - Products built specifically for whales (institutional pools, etc.)?
   - Pricing/fee tiers (discounts for large positions)?

**C. Institutions (Funds, DAOs, companies)**

**Questions:**
1. When did they start targeting institutions?
   - How long after retail launch (6 months? 2 years?)?
   - What triggered the shift (maturity, demand, regulatory clarity)?
   - Was this planned or opportunistic?

2. What products did they build for institutions?
   - Permissioned pools (KYC/AML compliant)?
   - Compliance/reporting tools (transaction exports, viewing keys)?
   - Custody integrations (which providers: Fireblocks, Anchorage, etc.)?
   - Risk management features (conservative LTV, whitelisting)?

3. What was the institutional GTM strategy?
   - B2B sales team (how many people, job titles, LinkedIn evidence)?
   - Partnerships (custody providers, law firms, compliance consultants)?
   - Thought leadership (conference talks, whitepapers, media appearances)?
   - Budget allocated (estimate from job postings, conference costs)?

4. Results?
   - What % of TVL is institutional (estimate)?
   - How many institutions onboarded?
   - Timeline: First institution → 10 institutions → 100 institutions?

**Output Section:** GTM Playbook by User Segment

---

### **3. BUSINESS MODEL & UNIT ECONOMICS**

**Goal:** Understand how the protocol makes money and whether it's sustainable

**Questions:**

1. **What's the revenue model?**
   - How does it charge? (fees, commissions, subscriptions, token emissions)
   - What's the effective fee rate? (not claimed — calculated: revenue / volume)
   - Who pays? (users directly, counterparties, exchange partners, token holders via dilution)

2. **Unit economics**
   - Revenue per transaction/user?
   - What % goes to operations vs token holders vs treasury?
   - Does the math work at current scale? At 10x? At 0.5x?
   - How much funding was raised vs how much revenue generated? (capital efficiency)

3. **Token economics (if applicable)**
   - Token utility — what is it used for?
   - Revenue sharing — real yield or inflationary emissions?
   - Holder concentration — who owns the supply? (check Dune, Etherscan top holders)
   - Liquidity — how much is in DEX pools? Is the token actually tradeable?
   - Buyback/burn mechanics — are they real and verifiable on-chain?

4. **Sustainability test**
   - If incentives stopped tomorrow, would usage continue?
   - Is the team funded by revenue or burning runway?
   - Team size vs revenue — does it add up?

**Data sources:** Dune Analytics, DeFiLlama fees page, Token Terminal, CoinGecko/CMC for token data, Etherscan for holder distribution

**Output Section:** Business Model & Unit Economics

---

### **4. CRITICAL ANALYTICS & DATA VALIDATION**

**Goal:** Separate verified facts from marketing claims. Never trust self-reported numbers without cross-referencing.

**Questions:**

1. **Real vs claimed metrics**
   - What does the protocol claim? (volume, users, revenue, TVL)
   - What do on-chain trackers show? (Dune, DeFiLlama, Token Terminal)
   - What's the discrepancy? Can it be explained? (e.g., off-chain routing invisible to on-chain trackers)
   - **Always check the Dune dashboard first** — it's the closest to ground truth

2. **Growth trajectory — honest assessment**
   - Is volume/usage accelerating, flat, or declining?
   - What's the current run rate vs peak? (% off peak)
   - Are trade counts growing even if volume is flat? (broadening base vs concentrating)
   - What external events correlated with spikes? (market rallies, narrative cycles)

3. **Red flags checklist**
   - [ ] Volume discrepancy between self-reported and tracked
   - [ ] Closed source code (for a trust-dependent product)
   - [ ] Unnamed critical partners or dependencies
   - [ ] Team size claims that don't match revenue/funding
   - [ ] Offshore incorporation without clear reason
   - [ ] Token holder concentration >50% in top 10 wallets
   - [ ] Thin trading volume on token (<$50K/day)
   - [ ] Aspirational targets disconnected from current trajectory
   - [ ] Data retention / privacy compromises (for privacy products)

4. **Honest verdict**
   - What's genuinely strong? (with evidence)
   - What's smoke and mirrors? (with evidence)
   - Bottom line: Is this a real business or a narrative dressed up in metrics?

**Output Section:** Critical Analytics (include metrics cross-reference table + red flags checklist)

---

### **5. RECURRING USAGE & RETENTION MECHANICS**

**Goal:** Understand why users come back (or don't)

**Questions:**

1. **What drives repeat usage?**
   - Is the core use case inherently recurring? (daily trading vs one-time swap)
   - What product features create stickiness? (staking, loyalty programs, expanding token support)
   - Community rituals? (weekly calls, AMAs, governance votes)
   - Financial incentives? (staking rewards, referral programs, airdrops)

2. **Retention signals (even without published data)**
   - Trade count growth vs user count growth — if trade count grows faster, existing users are coming back
   - Average transaction size trending down — suggests broadening from power users to recurring retail
   - Community size and engagement — Discord/Telegram activity, governance participation
   - Staking lock-up rates — what % of token holders are staked (proxy for commitment)

3. **What would make users leave?**
   - Better alternative appears
   - Incentives dry up
   - Security incident
   - Regulatory action
   - Product stagnation

4. **Product surface expansion**
   - Has the protocol expanded from one use case to multiple? (e.g., swap → payment links → API)
   - Does each new feature bring new users or retain existing ones?
   - What's on the roadmap that could change retention dynamics?

**Output Section:** Recurring Usage & Retention Analysis

---

### **6. INGREDIENTS OF SUCCESS**

**Goal:** Break down success into replicable components

**Questions:**

1. **What caused the breakthrough moment?**
   - Specific event/product/announcement that triggered growth spike?
   - Was it timing/market conditions or strategic execution?
   - External catalysts (competitor failure, regulatory change, macro trend)?
   - Can you pinpoint the inflection point (date, metric)?

2. **What were the key ingredients?**
   - Unique feature (what could users ONLY do here)?
   - Better economics (higher yields, lower fees — quantify difference)?
   - Better UX (lower friction, easier onboarding — specific improvements)?
   - Network effects (more users = better product how?)?
   - Community/governance (user ownership, active participation)?
   - Timing/luck (right place, right time)?

3. **Timeline: What came when?**
   - Month 0-3: Launch, early adopters (TVL: $X)
   - Month 3-6: First growth phase (TVL: $X)
   - Month 6-12: Scaling/breakout (TVL: $X)
   - Month 12-24: Maturity/institutional adoption (TVL: $X)
   - What were key milestones at each phase?

4. **What can we replicate vs what was non-replicable?**
   - **Replicable:** Features (can be copied), positioning (can be adapted), channel strategy
   - **Non-replicable:** Market timing (DeFi Summer 2020 won't happen again), first-mover advantage, lucky breaks
   - Which ingredients are **TABLE STAKES** (must-have to compete) vs **DIFFERENTIATION** (nice-to-have)?

**Output Section:** Ingredients of Success

---

### **7. USER BEHAVIOR & PRECEDENT VALIDATION**

**Goal:** Prove that demand/behavior actually exists (not just narrative)

**Questions:**

1. **Where do users currently solve this problem?**
   - What alternatives are they using today (competitors, workarounds)?
   - What are the costs/pain points of current solutions?
   - Evidence of existing volume/usage (on-chain data, public metrics, $ amounts)?

2. **Why would users switch?**
   - Better economics (how much better? 0.5% improvement vs 5% improvement)?
   - Better UX (10x better or marginally better)?
   - Unique capability (can't get elsewhere = forced switch)?
   - Risk reduction (security, privacy, compliance improvement)?

3. **What's the precedent?**
   - Has similar behavior been proven elsewhere (analogous markets)?
   - What's the TAM/volume of existing solutions (market size validation)?
   - Industry benchmarks (e.g., "DeFi users rebalance 3-7% of portfolio monthly" = precedent)?

4. **How do users currently behave?**
   - Usage frequency (daily, weekly, monthly)?
   - Transaction sizes (typical: $X, whale-sized: $Y)?
   - Retention patterns (sticky or high churn)?
   - **Data sources:** Dune Analytics dashboards, DeFiLlama charts, Token Terminal metrics, on-chain data, Nansen cohort analysis
   - Prioritize quantitative dashboards over opinion pieces or anecdotal claims

**Output Section:** User Behavior & Precedent Analysis

---

### **8. MARKET NARRATIVES & INFLUENCER CASCADE**

**Goal:** Understand what narratives helped the protocol and who amplified them

**Questions:**

1. **What market narratives helped the protocol?**
   - What broader market themes did it ride (e.g., "DeFi Summer", "privacy is the next frontier", "real yield")?
   - Which narratives were externally created (market trends it benefited from) vs internally created (the protocol's messaging)?
   - What claims/memes became associated with the protocol (e.g., "blue-chip DeFi", "the privacy Uniswap")?
   - How did the protocol position itself within existing narratives?

2. **Who started the narrative?**
   - Was there a single "patient zero" tweet/article (link, date)?
   - Or multi-threaded origins (multiple people independently)?
   - Timeline: When did each key influencer enter the conversation?

3. **What convinced early influencers?**
   - Technical merit (they understood the innovation)?
   - Financial incentive (they held bags, got paid, had partnerships)?
   - Ideological alignment (believed in the mission)?
   - Social proof (copied other influencers)?

4. **How did it cascade?**
   - Pattern: TA community → Builders → KOLs → Philosophers → Traders?
   - Who had the biggest impact (measured by price movement, volume spike, follower count)?
   - Was amplification organic or coordinated (evidence for/against)?
   - Multi-threaded (several origin points) or single-origin (one person started it)?

5. **What was the timeline?**
   - First mention (date, who, reach)
   - Amplification phase (dates, key tweets/posts)
   - Peak narrative saturation (when everyone was talking about it)
   - Decay (when narrative faded or became mainstream)

**Output Section:** Market Narratives & Influencer Timeline (include influencer timeline table)

---

### **9. OBJECTIONS & BLOCKERS (PRE-ADOPTION)**

**Goal:** Anticipate what will stop users from switching

**Questions:**

1. **What are users giving up by switching?**
   - Features lost (integrations, composability, liquidity depth)?
   - Costs incurred (gas fees, migration effort, learning curve)?
   - Risks introduced (new protocol = unproven, regulatory uncertainty)?

2. **What will users question/challenge?**
   - **Security:** "Has this been audited? What's the track record? What if it gets hacked?"
   - **Compliance:** "Is this legal? Can I prove source of funds for taxes? Will I get flagged?"
   - **Liquidity:** "Can I exit when I need to? What if there are no buyers? What's slippage on large trades?"
   - **Social proof:** "Who else uses this? Am I the guinea pig? What if it fails?"
   - **Trust:** "Who's behind this? What if they rug pull? Will it exist in 2 years?"

3. **What are the deal-breakers vs nice-to-haves?**
   - **CRITICAL blockers** (e.g., no audit = whales >$1M won't touch it)
   - **HIGH blockers** (affects decision but not fatal — may delay adoption)
   - **MEDIUM concerns** (can be addressed post-launch without killing conversion)

4. **How did [Protocol] address objections?**
   - What proof points convinced skeptical users?
   - What mitigation strategies worked?
   - What objections did they ignore (and did it matter)?

**Output Section:** Objections & Blockers (with severity ranking table)

---

### **10. STRATEGIC EVALUATION (4 CRITERIA FRAMEWORK)**

**Goal:** Score competitor strategies to understand confidence level

**Apply 4 criteria to each major strategy [Protocol] used:**

| Strategy | Market Size | Defensibility | Strengths Alignment | Precedent | Overall |
|----------|------------|--------------|-------------------|-----------|---------|
| [Strategy 1] | H/M/L | H/M/L | H/M/L | H/M/L | X/4 HIGH |
| [Strategy 2] | H/M/L | H/M/L | H/M/L | H/M/L | X/4 HIGH |

**For each criterion:**
- **Market Size:** How large was the addressable market? Growing or shrinking?
- **Defensibility:** What moats did it create (network effects, switching costs, regulatory barriers)?
- **Strengths Alignment:** Did it play to their advantages (team, tech, resources)?
- **Precedent:** Had similar strategies worked elsewhere? Or was this exploratory?

**Analysis Questions:**
1. Which strategies scored HIGH on all 4 criteria (low-risk, high-confidence)?
2. Which strategies had LOW precedent but succeeded anyway (what made them work despite risk)?
3. Which strategies SHOULD have worked (high scores) but failed (what went wrong)?

**Output Section:** Strategic Evaluation (include table + analysis)

---

### **11. FACT-CHECKING NARRATIVES**

**Goal:** Distinguish mythology from reality

**Questions:**

1. **What's the popular narrative?**
   - What do people CLAIM drove the success?
   - What's the "hero story" being repeated (e.g., "Single tweet caused 10x")?
   - Where is this narrative repeated (Twitter, articles, podcasts)?

2. **What's the actual evidence?**
   - **Primary sources:** Tweets with timestamps, on-chain data with dates, official announcements
   - **Contradictory evidence:** What disproves the popular narrative?
   - **Timeline validation:** Did X actually happen before Y (causation), or just at the same time (correlation)?

3. **Create fact-check table:**

| Claim | Evidence FOR | Evidence AGAINST | Verdict |
|-------|-------------|------------------|---------|
| "Arthur Hayes tweet caused rally" | Tweet on Oct 26, +30% same day | Naval tweet Oct 1 (+62%) came first, Hayes was late | PARTIALLY FALSE |
| "Users chose privacy over profit" | 1M ZEC stayed shielded during rally | 200K ZEC unshielded Jan 2026 → Binance | PARTIALLY FALSE |

4. **What's the corrected story?**
   - Narrative vs Reality side-by-side
   - Lessons: What can we learn from the gap between myth and truth?

**Output Section:** Critical Analysis (Fact-Checking)

---

### **12. DISTRIBUTION MECHANICS (PRODUCT TYPE MATTERS)**

**Goal:** Understand how product type affects distribution strategy

**Questions:**

1. **What product type is [Competitor]?**
   - Protocol (Aave, Uniswap) vs Wallet (MetaMask, Phantom) vs Infrastructure (Chainlink)?
   - B2B (enterprise sales) vs B2C (consumer marketing) vs B2B2C (platform/marketplace)?

2. **What distribution channels did they use?**
   - **For Protocols:**
     - Integrations (got listed on aggregators: Zapper, Zerion, etc.)
     - Developer adoption (SDKs, documentation, hackathons)
     - Liquidity mining (token incentives to bootstrap usage)
   - **For Wallets:**
     - Browser extension stores (Chrome, Firefox)
     - WalletConnect (appear on dapp connection lists)
     - Dapp partnerships ("recommended wallet")
     - Import from competitors (lower switching cost)
   - **For Infrastructure:**
     - Developer relations (technical documentation, support)
     - Integration partnerships (protocols building on top)
     - Conference presence (ETH Denver, Devcon)

3. **What's replicable for OUR product type?**
   - **Protocol → Wallet:** Can't do "get integrated everywhere," must BE the integrator
   - **Wallet → Protocol:** Can't be embedded, must win on UX/features/brand
   - **B2B → B2C:** Sales team vs marketing-led growth, different CAC structures
   - **Document:** What they did + How we'd adapt for our product type

4. **What ecosystem dependencies did they have?**
   - Did they rely on existing ecosystem (users, dapps, liquidity already there)?
   - What if that ecosystem doesn't exist for us (e.g., new chain with zero dapps)?
   - How do we adapt if we can't piggyback on existing distribution?
   - Alternative strategies when ecosystem is missing?

**Output Section:** Distribution Strategy & Product Type

---

### **13. OVERLAP ANALYSIS & APPLICATION TO US**

**Goal:** Map exactly where the analyzed protocol overlaps with our product and extract specific growth levers

**Questions:**

1. **User journey gap analysis**
   - Draw the user's current workflow with [Protocol]
   - Where does their journey end in a gap our product fills?
   - What does the combined workflow look like? (e.g., "Houdini breaks the link, {{PRODUCT}} keeps it broken")

2. **Shared user segment mapping**
   - For each user segment identified in Section 2, rate overlap: HIGH / MEDIUM / LOW
   - Which segments are using [Protocol] today that would also benefit from our product?
   - Are they the same user at a different point in their workflow, or genuinely different users?

3. **What does [Protocol] prove about our market?**
   - Volume/revenue validates demand exists? (quantify)
   - User behavior patterns that confirm our thesis? (e.g., "majority choose privacy when offered")
   - Pricing benchmarks — what do users pay for similar value?

4. **Growth levers unique to us**
   - What can we do that [Protocol] can't? (different product type, different tech, different partnerships)
   - What integration/funnel exists between their product and ours?
   - What category can we own that they don't? (e.g., "privately hold" vs "privately swap")

5. **Growth priorities (ranked)**
   - List actions ranked by expected impact and effort
   - Separate into: steal directly, adapt, and unique to us

**Output Section:** Overlap Analysis & Growth Playbook (include user journey diagram, segment overlap table, ranked growth priorities)

---

## Analysis Document Template

Save as: `docs/[protocol-name]-analysis-YYYY-MM-DD.md`

```markdown
# [Protocol Name] Analysis — Competitive Research

**Date:** [Date]
**Analyzed by:** [Your name/team]
**Purpose:** [Why we're analyzing this - e.g., "Understand Aave's whale acquisition to inform {{PRODUCT}} strategy"]
**Status:** Draft / Final

---

**TL;DR:** [2-3 sentences summarizing key findings]

**Key Takeaways:**
- [Learning 1]
- [Learning 2]
- [Learning 3]

---

## Table of Contents

1. [Origin Story & Trust Timeline](#1-origin-story--trust-timeline)
2. [GTM Playbook by User Segment](#2-gtm-playbook-by-user-segment)
3. [Business Model & Unit Economics](#3-business-model--unit-economics)
4. [Critical Analytics & Data Validation](#4-critical-analytics--data-validation)
5. [Recurring Usage & Retention Analysis](#5-recurring-usage--retention-analysis)
6. [Ingredients of Success](#6-ingredients-of-success)
7. [User Behavior & Precedent Analysis](#7-user-behavior--precedent-analysis)
8. [Market Narratives & Influencer Timeline](#8-market-narratives--influencer-timeline)
9. [Objections & Blockers](#9-objections--blockers)
10. [Strategic Evaluation](#10-strategic-evaluation)
11. [Critical Analysis (Fact-Checking)](#11-critical-analysis-fact-checking)
12. [Distribution Strategy & Product Type](#12-distribution-strategy--product-type)
13. [Overlap Analysis & Growth Playbook](#13-overlap-analysis--growth-playbook)
14. [What We Can Steal vs What to Avoid](#14-what-we-can-steal-vs-what-to-avoid)

---

## 1. Origin Story & Trust Timeline

**Founded:** [Date, by whom]
**Initial product:** [What it was]
**Pivot (if any):** [What changed, when, why]

**Trust Timeline:**

| Milestone | Date | TVL/Metric | Trust Level |
|-----------|------|-----------|-------------|
| Launch | [Date] | $X | Low |
| First $100M | [Date] | $100M | Medium |
| First $1B | [Date] | $1B | High |
| Institutional adoption | [Date] | $XB | Very High |

**Why they won vs competitors:**
- [Differentiation 1]
- [Differentiation 2]

**Why it won (the one thing):**

[One paragraph. The single core positioning insight that explains adoption. Not a feature list — the market gap they filled and the psychological barrier they removed. Confirm with on-chain data.]

---

## 2. GTM Playbook by User Segment

### Retail Users
**Acquisition channels:**
- [Channel 1: % of users, CAC, timeline]
- [Channel 2: % of users, CAC, timeline]

**Budget:**
- Token emissions: $X
- Cash spend: $Y
- Team: Z people

### Whales ($1M+)
**How they came:**
- [Organic vs BD, evidence]

**What they needed:**
- [Requirement 1]
- [Requirement 2]

### Institutions
**When:** [X months after launch]
**Products built:** [Institutional features]
**GTM strategy:** [Sales team size, partnerships, budget]

---

## 3. Business Model & Unit Economics

**Revenue model:** [How it charges, effective fee rate (calculated, not claimed)]

| Metric | Value | Source | Confidence |
|--------|-------|--------|------------|
| Cumulative volume | $X | Dune | HIGH/MED/LOW |
| Cumulative revenue | $X | Dune | HIGH/MED/LOW |
| Effective fee rate | X% | Derived | HIGH/MED/LOW |
| Revenue per transaction | $X | Derived | HIGH/MED/LOW |
| Funding raised | $X | [Source] | HIGH |
| Capital efficiency | Revenue / Funding = Xx | Derived | HIGH/MED/LOW |

**Token economics (if applicable):**
- Utility, revenue sharing, holder concentration, liquidity, buyback mechanics

**Sustainability verdict:** [Can this run without external funding?]

---

## 4. Critical Analytics & Data Validation

**Metrics cross-reference:**

| Metric | Self-Reported | Dune | DeFiLlama | Token Terminal | Verdict |
|--------|-------------|------|-----------|---------------|---------|
| Volume | $X | $X | $X | $X | REAL / INFLATED / UNKNOWN |
| Revenue | $X | $X | $X | $X | REAL / INFLATED / UNKNOWN |

**Red flags checklist:**
- [ ] Volume discrepancy
- [ ] Closed source
- [ ] Unnamed critical partners
- [ ] Team size vs revenue mismatch
- [ ] Offshore incorporation
- [ ] Token concentration >50% top 10
- [ ] Token illiquidity (<$50K/day)
- [ ] Aspirational targets disconnected from trajectory

**Honest verdict:** [What's real, what's smoke, bottom line]

---

## 5. Recurring Usage & Retention

**What drives repeat usage:**
- [Mechanism 1: inherent use case frequency]
- [Mechanism 2: stickiness features]
- [Mechanism 3: community/financial incentives]

**Retention signals:** [Trade count trends, avg size trends, community engagement]

**Verdict:** [Sticky or leaky? Power user base or one-time usage?]

---

## 6. Ingredients of Success

**Breakthrough moment:** [What caused growth spike]

**Key ingredients:**
1. [Ingredient 1 - replicable/non-replicable]
2. [Ingredient 2 - replicable/non-replicable]
3. [Ingredient 3 - replicable/non-replicable]

**Timeline:**
- Month 0-3: [Milestone, TVL]
- Month 3-6: [Milestone, TVL]
- Month 6-12: [Milestone, TVL]

---

## 7. User Behavior & Precedent Analysis

**Where users currently solve this:**
- [Alternative 1: volume, cost]
- [Alternative 2: volume, cost]

**Why users switched:**
- [Reason 1: quantified benefit]
- [Reason 2: quantified benefit]

**Precedent validation:**
- [Evidence of existing behavior/demand]
- [Industry benchmarks]

---

## 8. Market Narratives & Influencer Timeline
(Include if relevant — skip if not narrative-driven)

**Market narratives that helped:**
- [Narrative 1: e.g., "DeFi Summer 2020"]
- [Narrative 2: e.g., "Real yield over ponzinomics"]

**Influencer Timeline:**
- [Date]: First mention by [Person, reach]
- [Date]: Amplification by [Person, impact]
- [Date]: Peak saturation

**Cascade pattern:** [Single-origin vs multi-threaded]

---

## 9. Objections & Blockers

| Objection | Severity | How They Addressed |
|-----------|---------|-------------------|
| [Objection 1] | CRITICAL | [Mitigation] |
| [Objection 2] | HIGH | [Mitigation] |

---

## 10. Strategic Evaluation

| Strategy | Market Size | Defensibility | Strengths | Precedent | Overall |
|----------|------------|--------------|-----------|-----------|---------|
| [Strategy 1] | H | M | H | H | 3/4 HIGH |
| [Strategy 2] | M | H | M | L | 2/4 MEDIUM |

**Analysis:** [What worked, what didn't, why]

---

## 11. Critical Analysis (Fact-Checking)

| Claim | Evidence FOR | Evidence AGAINST | Verdict |
|-------|-------------|------------------|---------|
| [Claim 1] | [Sources] | [Contradictions] | TRUE/FALSE |

**Corrected timeline:** [Reality vs narrative]

---

## 12. Distribution Strategy & Product Type

**Product type:** [Protocol/Wallet/Infrastructure]

**Distribution channels used:**
- [Channel 1]
- [Channel 2]

**Adaptation for our product type:**
- [What we can copy]
- [What we must adapt]

---

## 13. Overlap Analysis & Growth Playbook

**User journey gap:**
```
[Protocol] user today:  [Step 1] → [Step 2] → [Gap our product fills]
With our product:       [Step 1] → [Step 2] → [Our product completes the flow]
```

**Shared user segments:**

| Segment | [Protocol]'s Role | Our Role | Overlap |
|---------|------------------|----------|---------|
| [Segment 1] | [What they do] | [What we do] | HIGH/MED/LOW |

**What [Protocol] proves for our market:**
- [Validated insight 1]
- [Validated insight 2]

**Growth levers unique to us:**
- [Lever 1 — why they can't do this]
- [Lever 2 — what we have that they don't]

**Growth priorities (ranked):**

| Priority | Action | Expected Impact | Effort |
|----------|--------|----------------|--------|
| 1 | [Action] | High/Med/Low | High/Med/Low |
| 2 | [Action] | High/Med/Low | High/Med/Low |

---

## 14. What We Can Steal vs What to Avoid

### STEAL These
1. [Strategy 1 - why it's replicable]
2. [Strategy 2 - how to adapt it]

### AVOID These
1. [Mistake 1 - why it won't work for us]
2. [Mistake 2 - context dependency]

### ADAPT These (Don't Copy Directly)
1. [Strategy that needs modification - how to adjust]

---

## Version History
| Date | Change |
|------|--------|
| [Date] | Initial analysis |
```

---

## Output Destination
- Save as `.md` in `docs/`
- Name format: `[protocol-name]-analysis-YYYY-MM-DD.md`
- Example: `aave-whale-strategy-2026-02-08.md`

---

## Tips for Effective Analysis

1. **Prioritize quantitative data over opinion:** Use dashboards (Dune Analytics, DeFiLlama, Token Terminal), on-chain data, and public metrics before relying on articles or tweets
2. **Use primary sources:** Tweets with links, on-chain data with block numbers, not just articles repeating claims
3. **Quantify everything:** Don't say "grew fast" — say "10x in 6 months from $100M to $1B"
4. **Leverage analytics platforms:**
   - Dune Analytics (user behavior, transaction volumes, retention)
   - DeFiLlama (TVL timelines, protocol comparisons)
   - Token Terminal (revenue, fees, active users)
   - Nansen (whale tracking, smart money flows)
5. **Timeline matters:** X before Y = potential causation, X and Y together = just correlation
6. **Product type determines strategy:** A protocol's playbook won't work for a wallet — adapt, don't copy
7. **Separate narrative from reality:** What people say happened vs what actually happened (check timestamps)
8. **Not everything is replicable:** Market timing (DeFi Summer) can't be recreated, but strategies can
9. **Focus on high-confidence learnings:** 4/4 criteria HIGH = steal this, 1/4 criteria HIGH = probably skip
10. **Use deep research agents when needed:** For complex multi-source analysis, invoke the deep-researcher agent to gather and synthesize data systematically
