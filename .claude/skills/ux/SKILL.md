---
name: ux
description: UX audits, competitive benchmarks, funnel analysis, craft proposals
---

# UX Craft

## Purpose
Optimize and elevate the {{PRODUCT}} experience across Chrome extension and mobile app. Two modes: audit mode finds friction and fixes it, craft mode designs moments of delight that make {{PRODUCT}} feel like the best consumer product in crypto.

**Philosophy:** Privacy already feels like a chore in every other wallet. {{PRODUCT}}'s UX should make privacy feel effortless AND premium. Every interaction should communicate "this is a product built by people who care about the details."

## Reads
- `docs/product/` — existing specs, flows
- `docs/feedback/` — user complaints mapped to flows
- `docs/market-research/` — competitor UX patterns
- `docs/strategy/` — what we're optimizing for (acquisition, retention, moat)
- `docs/product/master-prd-roadmap.md` — master PRD and roadmap (what's shipping, what's next)

## Writes
- `docs/product/` — UX audit reports, improvement specs, craft proposals

## Modes

### 1. UX Audit (Fix Friction)
Systematic review of a flow or screen. Run post-launch, then after every major release.

**Process:**
1. Define the flow to audit (onboarding, first shield, send, receive, etc.)
2. Walk every step, checking against heuristics below
3. Flag issues by severity: Blocker (user can't proceed), Friction (user hesitates or gets confused), Polish (works but feels rough)
4. For each issue, propose a fix with before/after description
5. Prioritize by: Blockers first, then friction on high-traffic flows, then polish

**Heuristic Checklist (Crypto-Specific):**

| #   | Heuristic                           | What to check                                                                                      |
| --- | ----------------------------------- | -------------------------------------------------------------------------------------------------- |
| 1   | Visibility of system status         | Does the user know what's happening? Transaction pending, proving in progress, shielding complete? |
| 2   | Match between system and real world | Do we use terms users know? Or internal jargon (records, auto-join, feemaster)?                    |
| 3   | User control and freedom            | Can they cancel, go back, undo? What happens if they close mid-transaction?                        |
| 4   | Consistency                         | Same patterns across extension and mobile? Same terminology?                                       |
| 5   | Error prevention                    | Do we prevent errors before they happen? (e.g., insufficient balance warning before send)          |
| 6   | Recognition over recall             | Does the user have to remember anything from a previous screen?                                    |
| 7   | Flexibility                         | Power users vs. new users. Are there shortcuts without cluttering the default?                     |
| 8   | Aesthetic and minimal design        | Is every element earning its space? Anything removable?                                            |
| 9   | Error recovery                      | When something fails, does the error message tell them what to do next?                            |
| 10  | Help and documentation              | Can they get unstuck without leaving the app?                                                      |
| 11  | Trust signals                       | Does the user feel safe? Privacy indicators, confirmation steps before irreversible actions        |
| 12  | Transaction confidence              | Does the user know their transaction went through? Do they know when funds will arrive?            |
| 13  | Wait time perception                | When proving takes time, does it feel like progress or a broken screen?                            |

**Output format:**

```markdown
# UX Audit: [Flow Name]

**Platform:** Extension / Mobile / Both
**Date:** [Date]
**Overall:** [X blockers, Y friction, Z polish items]

## Blockers
| Step | Issue | Impact | Fix |
|------|-------|--------|-----|

## Friction
| Step | Issue | Heuristic Violated | Fix | Effort |
|------|-------|--------------------|-----|--------|

## Polish
| Step | Issue | Fix | Effort |
|------|-------|----|--------|
```

---

### 2. Competitive UX Benchmark
Side-by-side comparison of how {{PRODUCT}} handles a flow vs. competitors.

**Process:**
1. Pick a flow (onboarding, send, swap, bridge, etc.)
2. Walk the same flow on 3-4 competitors (Phantom, Rainbow, Rabby, MetaMask, or whatever's relevant)
3. Screenshot or describe each step
4. Compare: where are they smoother? Where do we win? What patterns are they using that we're not?
5. Extract steal-worthy patterns (not features, patterns)

**Benchmark against two tiers:**
- **Crypto wallets**: Phantom, Rainbow, Rabby, MetaMask, Coinbase Wallet
- **Best consumer apps** (for craft inspiration): Apple Wallet, Cash App, Revolut, Arc browser

**Output format:**

```markdown
# UX Benchmark: [Flow Name]

## Flow Comparison
| Step | {{PRODUCT}} | [Competitor 1] | [Competitor 2] | Best in Class |
|------|--------|----------------|----------------|---------------|

## Where We Win
- [Advantage]: [Why it matters]

## Where We Lose
- [Gap]: [Who does it better and how]

## Patterns to Steal
- [Pattern]: [Who uses it, why it works, how we'd adapt it]
```

---

### 3. Funnel Analysis
Use analytics data to find where users drop off and why.

**Process:**
1. Pull funnel data from Amplitude for the flow being analyzed
2. Identify the biggest drop-off points (where do we lose the most users?)
3. For each drop-off, hypothesize why (confusing UI, too many steps, trust issue, technical failure)
4. Cross-reference with feedback data (are users complaining about this step?)
5. Propose fixes ordered by: drop-off magnitude x fix effort

**Output format:**

```markdown
# Funnel Analysis: [Flow Name]

**Period:** [Date range]
**Total entries:** [N]
**Completion rate:** [X%]

## Drop-off Points
| Step | Users In | Users Out | Drop-off % | Likely Cause | Fix | Effort |
|------|----------|-----------|------------|--------------|-----|--------|

## Biggest Opportunity
[The one fix that would move the needle most, with math]
```

---

### 4. App Store Optimization
Chrome Web Store and App Store (iOS/Android) presence. Screenshots, description, ratings, reviews.

**Process:**
1. Audit current listing: screenshots, description, keywords, ratings
2. Compare against top-performing crypto wallet listings
3. Identify gaps: missing screenshots, weak description, missing keywords
4. Draft improvements

**What to check:**
- Screenshots: Do they show the key value prop in the first 2 screenshots? (Privacy, multi-asset, yield)
- Description: Does the first sentence hook? Does it hit search keywords?
- Ratings: What are the 1-star reviews saying? Are we responding?
- Keywords: What are competitors ranking for that we're not?

---

### 5. UX Craft (Design Delight)
This is the layer above fixing problems. This is making {{PRODUCT}} feel like nothing else in crypto.

**Principles define what we believe.** This section defines how to implement it. Every proposal must pass the whale test.

**Craft Dimensions:**

| Dimension | Implementation spec |
|-----------|-------------------|
| Haptics | See Haptic Vocabulary table below |
| Sound | Subtle confirmation on receive, deliberate silence on background operations. Never on routine actions. |
| Motion | See Proving Latency Playbook below. {{PRODUCT}} animation wrapping assets on first shield. |
| Visual privacy | Softer gradients, blurred transitions for private state. Subtle privacy aura around shielded balances. No lock icons. |
| Copywriting | "{{PRODUCT}}ed. Nobody saw that." not "Transaction complete." Personality in microcopy. |
| Materiality | Strong typography, slight glass morphism, refined dark mode, careful shadows, subtle depth hierarchy. |
| Moments of surprise | Milestone celebrations (first shield, $10K shielded). Never on routine transactions. |

**Haptic Vocabulary:**

| Action | Haptic Pattern | Why |
|--------|---------------|-----|
| Send initiated | Light tap | Acknowledges the commitment |
| Transaction confirmed | Soft impact | Success feels solid, not flashy |
| Transaction failed | Rigid double-tap | Distinct from success, signals attention needed |
| Proof generating | No haptic (motion handles this) | Don't vibrate during a wait |
| {{PRODUCT}} complete | Subtle warm pulse | Privacy activation feels protective |
| Error state | Sharp buzz | Immediately distinct from any positive feedback |

**Proving Latency Playbook:**
{{CHAIN}} proving takes time. Design perceived speed, not actual speed.

1. Break the wait into named steps with micro-progress animations
2. Use optimistic UI where possible (show confirmation immediately, finalize in background)
3. If wait exceeds 5 seconds, add context ("Privacy proofs take a moment. Your transaction is secure.")
4. Never use a generic spinner. Named steps feel 2-3x faster than unnamed waits.
5. If wait exceeds 30 seconds, allow the user to leave the screen with a "We'll notify you" option

**Process:**
1. Map the emotional journey of a flow. Where does the user feel anxiety? Anticipation? Relief? Accomplishment?
2. For each emotional moment, propose a craft intervention from the dimensions above
3. Prioritize by: high-emotion moments first (sending money, shielding for the first time, seeing balance after shield)
4. Spec each craft intervention with enough detail for eng/design to implement
5. Test: does this slow down the flow? Does it annoy on the 100th use? Does it work with accessibility needs?

**Craft Intervention Format:**

```markdown
## [Moment Name]

**Flow:** [Which flow, which step]
**Emotion:** [What the user feels here: anxiety, anticipation, relief, accomplishment]
**Current experience:** [What happens now]

**Proposed craft:**
- **Haptic:** [Pattern and intensity]
- **Sound:** [Description, when it plays, duration]
- **Motion:** [Animation description]
- **Copy:** [Exact microcopy]

**Rules:**
- Must not add latency to the flow
- Must not annoy on repeat use (the 100th time matters more than the first)
- Must degrade gracefully (muted phone, screen reader, old device)
```

**Emotional Journey Map:**
For any flow, map these moments:

| Moment | Emotion | Opportunity |
|--------|---------|-------------|
| Initiating action (tap send/shield) | Commitment, slight anxiety | Confirm the action with subtle haptic |
| Waiting (proving, confirming) | Uncertainty, impatience | Progress indication that feels alive, not stuck |
| Success | Relief, accomplishment | Celebration proportional to the action |
| Failure | Frustration, worry | Immediate clarity on what happened and what to do |
| Checking balance | Curiosity, reassurance | Make the number feel real and trustworthy |

---

## When to Run Each Mode

| Mode                   | When                                          | How often                             |
| ---------------------- | --------------------------------------------- | ------------------------------------- |
| UX Audit               | After launch, after every major release       | Every 2-4 weeks                       |
| Competitive Benchmark  | When a competitor ships UX changes, quarterly | Quarterly                             |
| Funnel Analysis        | 1 week post-launch, then ongoing              | Weekly for first month, then biweekly |
| App Store Optimization | Pre-launch, then monthly                      | Monthly                               |
| UX Craft               | After friction is fixed in a flow             | Per flow, as capacity allows          |

**Order matters:** Audit first (fix what's broken), Funnel second (fix what's leaking), Craft third (elevate what works). Don't polish a broken flow.

## Trigger
"run ux audit", "ux review [flow]", "benchmark [flow] against competitors", "funnel analysis for [flow]", "app store audit", "craft proposals for [flow]", "ux craft", `/ux`
