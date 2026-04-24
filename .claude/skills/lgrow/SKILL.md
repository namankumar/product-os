---
name: lgrow
description: Weekly LinkedIn content engine. Draft posts and comments
---

# LinkedIn Growth Skill

## Purpose

Weekly content engine for {{YOUR_NAME}}'s LinkedIn. LinkedIn is a credibility channel, not a growth engine. High quality, leadership framing. 3 posts/week baseline, 4 when you have strong content. Output is always drafts. {{YOUR_NAME}} posts.

**Primary goal: profile views, DMs, connection requests.** Not impressions, not engagement rate, not followers. Those are lag indicators. High-intent actions are the signal.

## Reads
- `.claude/skills/utils/voice.md` -- baseline voice, anti-LLM rules (always load before drafting)
- `projects/growsocial/planning/quality-gates.md` -- shared pre-generation gates (always load before drafting)
- `projects/growsocial/li/evals.md` -- LinkedIn-specific evals (AI-detection gate + shared + platform-specific + batch + enforcement). **This is the eval file for LinkedIn. Not the shared evals.md.**
- `projects/growsocial/li/voice.md` -- LinkedIn-specific overrides (always load before drafting)
- `projects/growsocial/li/strategy.md` -- positioning, pillars, algorithm rules, crypto-to-fintech translation
- `projects/growsocial/li/tracker.md` -- what's working, metrics
- `projects/growsocial/planning/bank.md` -- shared experience bank
- `projects/growsocial/planning/content-prep.md` -- shared content prep (Miner, Off-platform, assignment sheet)
- `cache/lgrow/*.md` -- prior weekly outputs for continuity
- `cache/xgrow/*.md` -- for cross-pollination (primary content source)
- `projects/growsocial/x/performance.md` -- X engagement data for identifying top performers
- `projects/growsocial/li/hooks.md` -- hook type library with {{YOUR_NAME}}'s own hook lines (read in Step 4)
- `projects/growsocial/li/formats.md` -- text post length targets by hook type (read in Step 4)
- `projects/growsocial/li/formulas.md` -- mandatory 6-step post formula (read in Step 4)
- `context/room/people/*.md` -- on-demand, if drafting content about people {{YOUR_NAME}} works with

## Writes
- `cache/lgrow/[YYYY-MM-DD].md` -- weekly output (3 posts)
- `projects/growsocial/li/tracker.md` -- weekly metrics update

## Execution

### Step 1: Cross-pollinate from X (~5 min)

**Run Step 1 and Step 2 simultaneously.** Launch the content-prep agents (Miner + Off-platform from Step 2) immediately when starting Step 1. Read last 5-7 X daily files while Miner and Off-platform scan in parallel. Both complete before Step 3 begins.

This is the primary content source. Read the last 5-7 X daily files. Identify content that:
- Got high engagement (views, likes, reply-backs) if performance data exists
- Fits LinkedIn's audience (not crypto-native, not too terse)
- Can be expanded with leadership framing ("I led this" not "I shipped this")

For each candidate, draft a LinkedIn-native version:
- Expand from 1-3 sentences to 150-300 words
- Add team context ("we" where appropriate)
- Frame through judgment and decision-making, not speed and scrappiness
- Translate crypto terminology to fintech (see strategy.md translation table)

Target: 1-2 cross-pollinated posts per week.

**Two sources inside Step 1 — read both from the xgrow cache:**
1. **{{YOUR_NAME}}'s own drafts** — replies and originals that performed well or have strong substance. Expand with leadership framing. **The fix and The reframe plays adapt especially well to LinkedIn.** The fix: a one-line X solution becomes a 150-200 word post — open with "I see this constantly," add one sentence of when {{YOUR_NAME}} hit the same problem, state the fix clearly, close with what it means for teams. The solution is still the hook. The reframe: an X reply operating on a structural layer others missed becomes "The conversation about [X] is missing [insight]" — expand with why the hidden layer changes the analysis, what leaders should actually be thinking about. Both convert well because LinkedIn's audience rewards people who see what others don't.
2. **OP posts in the xgrow cache** — the external posts {{YOUR_NAME}} was replying to (from @karpathy, @LiorOnAI, @emollick, @simonw, etc.) are already logged with full text. These OPs often contain the insight worth adapting for LinkedIn. Extract the core claim, match to an experience bank entry, reframe with leadership context. The xgrow scanner already did the discovery work — don't re-scan X.

**External OP extraction process:**
1. For each OP logged in the xgrow daily files, check topic fit first. Only pull OPs that map to LinkedIn's pillars:
   - **AI leadership fit:** AI changing how teams work, what the human's job becomes, agent architecture, prompt quality as bottleneck, team size/leverage, what AI can't do — anything a CEO/VC would find operationally relevant
   - **Fintech/market fit:** financial infrastructure, market dynamics, privacy-as-product — translatable without crypto-native framing
   - **Skip:** crypto-native takes (ZK, UTXOs, chain comparisons), pure builder scrappiness posts, anything that requires the reader to already be in crypto to understand the point
2. For topic-fit OPs: extract the core insight in one sentence
3. Match to an experience bank entry. No match = skip. The OP provides the framing; {{YOUR_NAME}}'s experience provides the substance.
4. Apply LinkedIn leadership framing: what decision does this inform for a CEO/VC? What does it mean for running a team?
5. The LinkedIn draft should not be traceable back to the OP. The insight transfers. The source doesn't.

**Reference example (already in xgrow cache 2026-03-07):** @karpathy autoresearch post → core insight: "your job becomes programming the programmer, the bottleneck shifts from researcher hours to prompt quality" → bank match: #64 (systems thinking as AI advantage) → LinkedIn angle: A2 (PM job splitting) or A5 (what AI can't do).

### Step 2: Run content prep

**Already running in parallel with Step 1.** If launched simultaneously with Step 1, results should be available by the time X file cross-pollination candidates are identified.

Run `projects/growsocial/planning/content-prep.md` with LinkedIn as the platform context:
- **Phase 0:** Build assignment sheet (available experience bank entries, formats, draft bank entries). Use `cache/lgrow/*.md` as prior outputs for cooldown bans.
- **Miner:** Scan `docs/` for sharp claims a VP or director would find useful. Match to experience bank entries.
- **Off-platform:** Scan Tier 1 (builder canon) and optionally Tier 2 (Reddit/HN/Lobsters) for content fuel that fits LinkedIn's leadership register.

This produces material alongside cross-pollination. Between cross-pollinated posts and content-prep output, fill all 3 slots.

### Step 3: Fill remaining slots from experience bank

If cross-pollination + content prep don't fill all 3 slots, draft natively from the experience bank. Prioritize:
- **AI leadership entries (map to A1-A8 sub-angles):** #52 (spec process → A4), #55 (job changed shape → A2/A3), #56 (old process obsolete → A8), #57 (4 parallel agents → A4), #58 (moat → A1/A8), #59 (alignment → A5), #64 (systems thinker → A2), #65 (AI output is a draft → A7), #66 (domain expertise → A5), #67 (rules and evals → A7), #68 (adversarial evals → A7), #69 (batch failure modes → A7), #70 (first-draft failure modes → A7), #71 (parallel agents staggered handoff → A4/A8), #72 (what agents miss by feel → A7), #73 (timing constraint → A7/A8), #74 (ban lists as creativity forcing → A2)
- **AI content system entries (meta-level, high differentiation):** #75 (topic before the post → A2), #76 (creator research as technique extraction → A2/A7), #77 (performance data closed the loop → A1/A8), #78 (what "AI wrote this" actually means → A7), #79 (cross-platform content arbitrage → A4/A8), #80 (building the system generates stories → A7), #81 (skill file quality audit → A7)
- **LinkedIn-only entries:** career arc (#32, #44, #48), team leadership (#35, #13, #14), personal (#60, #61, #62, #63)

Optionally scan `docs/` for carousel material: frameworks, competitive breakdowns, or strategy docs that could become visual step-by-steps.

### Step 4: Format Determination (per post)

Runs after all source material is identified (Steps 1–3). Before any draft agent launches. Mandatory for all text posts. ~2 min per post.

Read `li/hooks.md`, `li/formats.md`, and `li/formulas.md` before running this step.

For each slot, determine and record:

1. **Hook type** (from `hooks.md`) — binary/contrarian for AI leadership slots (highest early-reaction driver), observation/confession appropriate for personal/fintech. Use the hook type → angle mapping table in hooks.md to match.
2. **Hook line draft** — write one candidate line. Test against LI9: would a reader comment "agree" or "disagree" on this line alone? If the best answer is "interesting" — rewrite. Binary and contrarian hooks are highest priority for AI leadership slots. If you cannot write a passing hook line for this content, swap the bank entry now. Do not draft hoping the hook improves.
3. **Length target** (from `formats.md`) — determined by hook type.

The draft agent receives hook type + hook line + length target as part of its brief. The hook line is locked — the agent builds the body to support it using the formula in `formulas.md`.

---

### Step 5: Draft posts (3 parallel agents)

**Before launching agents, complete all assignments upfront** (pillar, format, experience bank entry for each of the 3 slots). This prevents agents from conflicting over the same bank entries.

**Pre-assignment checks (run before launching agents):**

**Pillar check (data-driven weights as of 2026-03-06):**
- **67% AI leadership (2 of 3 core slots: Tue + Wed).** Primary growth driver. Drives profile views, recruiter DMs, high-intent connections. Every AI leadership post must have at least one specific operational detail (LI6) — a number, a daily ritual, a named behavior. "Four agents run in parallel before I open Slack" is the bar.
- **~20% Personal + career arc (Thursday, alternating weeks).** Untested but hypothesis: AI-post reach + better comment depth. Tolerates weekend Wave 2.
- **~13% Fintech/privacy (Thursday, alternating weeks).** Audience-fit post, not growth. Gets denser reaction rate but limited reach. Every other week only. Needs weekday Wave 2 for recruiter conversion.
- Monday is an optional 4th slot (AI leadership only). Only use if content is as strong as the best AI post that week. Never force it.

**Angle check (for AI leadership slots — read `li/strategy.md` § AI Leadership Sub-Angles):**
- Assign one of the 8 sub-angles (A1-A8) to each AI leadership slot before drafting.
- No two AI leadership posts in the same week use the same angle.
- The angle determines the opening technique and which creator to pattern-match:
  - A1 (team economics) → Dharmesh technique: uncomfortable claim, no hedging
  - A2 (PM job splitting) → Shreyas technique: "something I've noticed after working at X, Y, Z"
  - A3 (function replacement) → Greg technique: "I'm not hiring X. Here's what replaced it."
  - A4 (specific ritual) → Allie technique: bold the numbers, make the ritual visual
  - A5 (what AI can't do) → Greg technique: "I tried to replace X. Didn't work. Here's why."
  - A6 (hiring changed) → Greg + Dharmesh: who you hire now vs before
  - A7 (calibration failures) → Greg: builder honesty, specific failure + fix
  - A8 (speed as weapon) → Dharmesh: state the math, don't explain it
- Include the assigned angle (e.g., "Angle: A4 — Specific ritual") in each post's header in the output file.
- **Check prior week's cache file** for angle usage. Avoid repeating the same angle two weeks in a row.

**Launch all 3 draft agents simultaneously.** Each agent receives:
- Its assigned slot (Post 1/2/3), pillar, format, experience bank entry, and **assigned angle (A1-A8 with creator technique)**
- **Hook type, hook line (locked), and length target** from Step 4 — build the body to support the hook using the formula in `formulas.md`
- The full draft content (cross-pollinated source or experience bank entry text)
- voice.md + quality-gates.md + li/voice.md context
- Full eval suite from `projects/growsocial/li/evals.md` (AI-detection gate + shared 1-15 + LI1-LI9)

**Each agent must also apply these quality rules during drafting (in addition to evals):**
- **Bold 3-5 key phrases per post.** LinkedIn is a scroll-and-scan platform. Bold catches eyes. Numbers, specific rituals, named behaviors. Not full sentences — phrases. (From li/voice.md Post 3 calibration.)
- **One wit line per post, placed mid-post.** Not a punchline. Not sarcasm. The line that makes a smart person smirk. Understated observation, compression, or self-aware absurdity. (From li/voice.md Wit section.)
- **Credibility signal in first 50 words.** Embed one proof point naturally — not a bio, not a credential list. "At {{COMPANY}}, we're building..." / "I've shipped 5 products, 4 failed..." / "Running product at a YC company means..." (From strategy.md Credibility Signal section.)
- **Hook is the first 2 lines before "see more."** The hook must create tension, contradiction, or a specific number. Not context-setting. Not "At {{COMPANY}}, we've been building..." Open with the observation or the claim.
- **Takeaway embedded in story, not stated.** The insight lands when the reader pictures the observation. Don't announce it. Don't say "the lesson here is." (From strategy.md Edu-tainment Rule.)

Each agent drafts independently, runs all per-draft evals (AI-detection gate first, then shared 1-15, then LI1-LI9), rewrites failures, and returns a passing draft with full eval annotation block. **No draft enters the weekly file until every per-draft eval passes.**

**Eval gate (MANDATORY — read `projects/growsocial/li/evals.md` and follow its process exactly):**

1. **AI-detection gate (LI-AI1 through LI-AI6)** — run first. If any fail, regenerate before running other evals.
2. **Shared evals (1-15)** — depth, anti-slop, authority, profile click, growth mode, density, awe, screenshot line, gap, insider detail, stealable idea, experience bank, factual accuracy, no borrowed authority.
3. **LinkedIn-specific evals (LI1-LI9)** — voice match, hook, crypto translation, comment engages OP, leadership frame, **specific operational detail (LI6 — mandatory for all AI leadership posts),** position-forcing hook (LI9 — mandatory for all posts).

**Every single per-draft eval must pass. If ANY eval fails, regenerate the entire draft and re-run ALL evals from scratch.** Do not patch one line. Do not assume other evals still hold after a rewrite. Include the full eval annotation block (defined in `li/evals.md` § Enforcement) on every draft before it enters the output file.

**After all 3 agents return:** The main agent runs batch evals B1-B4 sequentially across the full set (ending variety, cadence variety, no batch arc, register variety). If batch evals fail, rewrite one draft at a time and re-run batch evals after each rewrite.

**Content angle bonus (LA1-LA4):** For prioritization, not pass/fail. Posts hitting 2+ angles are strongest. If a draft hits 0 angles, don't finalize — look harder for the angle in the experience bank entry or reframe the observation. A post with no angle is a post without a point of view.

**LinkedIn-specific kills:** Full kills table in `li/voice.md` § LinkedIn-specific kills. Hard minimums: no external links in post body (-60% reach), no engagement bait (algorithm suppresses), no "I'm thrilled/excited/humbled," no hashtag walls.

### Step 5: Assemble weekly file

Write to `cache/lgrow/[YYYY-MM-DD].md`:

```
# LinkedIn Weekly -- [DATE]

## Pillar Mix This Week
- AI leadership: X/3 posts (target: 2)
- Fintech/privacy: X/3 posts (target: 0-1, every other week)
- Personal + career arc: X/3 posts (target: 1)

## Posts

### Post 1
**Pillar:** [AI leadership / Fintech-privacy / Personal]
**Angle:** [A1-A8 — name / N/A for non-AI-leadership]
**Format:** [Text / Carousel]
**Experience bank #:** [number]
**Posting day:** [Mon (optional) / Tue / Wed / Thu]
**Source:** [cross-pollinated from X daily YYYY-MM-DD / experience bank / miner]
**Content angles:** [LA1/LA2/LA3/LA4 hits]

[Draft]

---

### Post 2
[...]

### Post 3
[...]

## Cross-Pollinated from X
[Which X posts were adapted, with source dates and original engagement if known]

## Notes
[Observations, what to try next week]
```

**Estimated time:** Sequential approach ~20-25 min. With parallel drafts + parallel Step 1/2: ~8-12 min. Savings: 3 drafts in parallel (~8 min), Step 1/2 overlap (~2 min).

### Step 6: Update tracker

If performance data is available, append to `projects/growsocial/li/tracker.md`. **Primary metrics first:**
- Profile views (week-over-week change)
- DMs received (who, from where)
- Connection requests
- Recruiter outreach
- Post impressions (secondary)
- Engagement rate (secondary)
- Follower count (lag indicator, last)

---
*Created: 2026-03-01*
*Updated: 2026-03-03 -- Restructured: removed comment scanning/targeting, made cross-pollination primary source, reduced to 3 posts/week, updated pillar weights to 50/20/30.*
*Updated: 2026-03-07 -- Data-driven updates from Week 2 performance: pillar weights to 67/13/20, cadence to Mon-Thu (Mon optional), primary metrics shifted to profile views/DMs/connections, LI6 (specific operational detail) added as mandatory eval, tracker update order reversed to lead with high-intent actions.*
*Updated: 2026-03-07 -- Quality pass: 8 AI leadership sub-angles (A1-A8) added to Step 4 pre-assignment with creator technique mapping; angle added to agent briefing and output template; bold phrases, wit, credibility signal, hook, and takeaway rules added to agent drafting instructions; experience bank entry list updated to include #64-67; content angle bonus guidance sharpened (0 angles = don't finalize); LinkedIn kills condensed to reference li/voice.md; prior-week angle check added.*
