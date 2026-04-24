---
name: xgrow
description: Daily X/Twitter content engine. Find posts, draft replies and originals
---

# X Growth Skill

## Purpose

Daily content engine for growing @namank on X. Scans X for high-engagement posts in target niches, drafts replies and original content, tracks what works. Output is always drafts. {{YOUR_NAME}} posts.

## Reads
- `projects/growsocial/planning/content-prep.md` — shared content prep (Phase 0 + Miner + Off-platform)
- `.claude/skills/utils/voice.md` — baseline voice, anti-LLM rules (always load before drafting)
- `projects/growsocial/planning/quality-gates.md` — shared pre-generation gates (always load before drafting)
- `projects/growsocial/x/plays.md` — full playbook: every play with spec, tier, evals, generation brief. **Read this before Phase 1.5. This is the primary generation reference.**
- `projects/growsocial/x/common-evals.md` — universal evals (U1-U4), play-specific evals (P1-P11), batch evals (B1-B6). Referenced by plays.md. **Pass to adversarial eval agents.**
- `projects/growsocial/x/voice.md` — X-specific voice overrides on top of voice.md
- `projects/growsocial/x/algorithm.md` — X algorithm analysis from source code (ranking signals, penalties, reply mechanics)
- `projects/growsocial/x/inspiration-bank.md` — viral tweet examples organized by play. Pass 2-3 examples to draft agents alongside play spec excerpt.
- X/Twitter (live, via agent-browser)
- Reddit, Hacker News, Lobsters (via agent-browser) — off-platform content fuel, not engagement
- `projects/growsocial/x/strategy.md` — positioning, pillars, target accounts, algorithm-driven rules
- `projects/growsocial/planning/weekly-theses.md` — thesis bank for weekly thesis rotation (Step 6c)
- `projects/growsocial/x/performance.md` — account metrics (top half) + per-tweet raw data (bottom half)
- `projects/growsocial/x/analysis.md` — cumulative analysis (findings, patterns, recommendations)
- `cache/xgrow/*.md` — prior daily outputs for continuity (most recent)
- `context/room/people/*.md` — on-demand, if drafting content about people {{YOUR_NAME}} works with

## Writes
- `cache/xgrow/[YYYY-MM-DD].md` — engagement targets + drafted content
- `projects/growsocial/x/analysis.md` — written by reverse-engineering (see `planning/reverse-engineering.md`), read-only for daily runs
- `projects/growsocial/x/performance.md` — append raw data after scrapes + weekly account metrics update

## Prerequisites

1. agent-browser must be installed (`npm install -g agent-browser`)
2. Auth state saved at `~/.agent-browser/x-auth.json` (run `--headed` to re-login if expired)

## Execution Architecture (parallel agents)

The skill runs in 4 phases. **Phases 0 and 1 run simultaneously.** Phase 2 uses parallel drafting agents with staggered launch. Phase 3a uses parallel eval agents.

### Phase 0: Orchestrator setup (main agent, ~2 min) — runs in parallel with Phase 1

**Step 0a: Read feedback from last 3 daily files (do this first, before launching Phase 1).**
Read `cache/xgrow/` last 3 files. Extract any `## Feedback` sections. Carry these signals forward:
- Plays with negative feedback → deprioritize this session (skip or reduce to 1 slot)
- Plays with overperformance notes → prioritize in Phase 1.5 assignment
- Promotion notes ("promote [play name]") → update that play's tier in `x/plays.md` from T2→T1 before assigning

**Step 0b: Launch Phase 1 agents (no waiting).** Then, while they scan, run content-prep (`projects/growsocial/planning/content-prep.md`) with X-specific inputs:
- Prior outputs: `cache/xgrow/*.md` (last 5 for angles, last 3 for play rotation)
- Strategy: `projects/growsocial/x/strategy.md`
- Voice: `projects/growsocial/x/voice.md`
- Playbook: `projects/growsocial/x/plays.md` (for assignment sheet)

This produces an **assignment sheet**: which plays and experience bank entries are available (not banned, not over-rotated). Held by the main agent for Phase 1.5. Phase 1 scanning takes ~3 min, so the assignment sheet is ready before Phase 1.5 begins.

### Phase 1: Gather raw material (5 parallel agents)

Launch all five simultaneously:

| Agent | Session | What it does | Tools | Returns |
|---|---|---|---|---|
| **Scanner A** | `x-scan-a` | Daily 8 + Tier 1 bench. Search, extract post data (Step 3), score (Step 4). | agent-browser (`x-auth.json`) | Ranked posts with full data, **post URL**, scores, action. |
| **Scanner B** | `x-scan-b` | Tier 2 + Tier 3 handles. Same extract + score. | agent-browser (`x-auth.json`) | Same format as Scanner A. |
| **Scanner C** | `x-scan-c` | Topic keyword searches + narrative hijack scan + trending topics + article intelligence. | agent-browser (`x-auth.json`) | Ranked posts (same format as Scanner A) **+ Trending Topics section + Article Intelligence section** (see below). |
| **Miner** | — | Shared step from `planning/content-prep.md`. Scan `docs/` for sharp lines, cross-reference with experience bank. | Read, Glob, Grep | List of (mined line + matched experience bank entry) pairs, tagged with source file |
| **Off-platform** | `offplatform` | Shared step from `planning/content-prep.md`. Scan Reddit, HN, Lobsters via agent-browser. Read Tier 1 local sources. | agent-browser (no auth), Read | List of content fuel items with **source URL** and matched experience bank entries. |

Each scanner uses its own `--session` name. No browser state collision.

**Inspiration bank collection (all scanners):** While scanning, if a post has >50K views and cleanly maps to a play format, save it to `projects/growsocial/x/inspiration-bank.md` under the matching play section. Capture: handle, views, date, format (statement/long-form tweet), dimensions (case, person, structure), full text, and one sentence on why it works. This grows the bank organically. Don't slow down scanning for this — append entries at end of scanner run. Seed accounts for initial population are listed in the inspiration bank file — these are structure references, separate from voice models in strategy.md.

**Staggered handoff (don't wait for all 5):**
- As soon as **any Scanner** returns, the main agent can begin assigning reply targets from those results.
- **Miner + Off-platform** results are only needed for original posts.
- Reply drafting agents launch as scanner results arrive. Original drafting agents launch once Miner/Off-platform return.

**Scanner C: Trending Topics output** (additional to ranked posts)

After scanning, Scanner C outputs a `## Trending Topics` section:
- 3-5 topics being actively debated across all scanned posts this session (not just Scanner C's handles — synthesize across all scanner results)
- For each topic: what the debate is about, which side is winning, and **which play(s) could produce a strong original on this topic** (e.g., "AI replacing junior devs" → Accusation, The reframe, The nobody-talks-about)
- Flag any topic not yet covered in the last 7 days of daily files — these are first-mover opportunities

Main agent reads Trending Topics at Phase 1.5 to check if any topic warrants a play that wasn't naturally called by the scanner results. One original slot per session can be designed around a trending topic if the bank has a matching entry.

**Scanner C: Article Intelligence output** (additional to Trending Topics)

Scanner C actively hunts for article-grade content every session. This feeds xarticle topic selection and keeps the inspiration bank stocked with long-form references.

**What to find:**
1. **High-performing articles** (X articles, not tweets): Search for recent articles with high bookmark counts or engagement. Look for articles from accounts in all tiers + keyword searches. Capture 1-2 per session: handle, views, bookmarks, topic, structure used (Investigation or Framework per xarticle spec), what made it work, full URL.
2. **Meta conversations** — what people are talking ABOUT, not just what they're saying. Look for: "everyone's been posting about...", "the discourse this week is...", quote-tweet pileups where 5+ accounts are riffing on the same post, reply sections where the replies became the content. These are the cultural moments that make articles timely.
3. **Emerging narratives** — topics gaining momentum but not yet saturated. A topic with 3-4 posts from different accounts in the same 24 hours is emerging. A topic with 20+ posts is saturated (too late for an article, fine for a reply). The sweet spot for articles is early enough to be a reference, not so early there's no audience.

**Output format** (appended to Scanner C results):
```
## Article Intelligence

### Articles Found
- [handle] — [topic] — [views/bookmarks] — [structure: Investigation/Framework] — [why it works] — [URL]

### Meta / Cultural Moments
- [topic] — [who's talking about it] — [which side is winning] — [saturation: emerging/peak/saturated] — [article angle if emerging]

### Narrative Momentum
- [topic] — [evidence: N posts from N accounts in N hours] — [{{YOUR_NAME}}'s angle if bank has matching entries]
```

Main agent passes the Article Intelligence section to xarticle when invoking it (Step 7j). Even on non-article days, this output accumulates context — the main agent logs the best items to `projects/growsocial/x/article-topics.md` (persistent pipeline) and to the daily file under `## Article Intel` so xarticle has a running feed of what's been spotted across sessions.

---

### Phase 1.5: Call a play (replaces "assign format")

Before launching any drafting agents, the main agent calls plays from `x/plays.md` for each draft slot.

**Inputs to Phase 1.5:**
- Scanner A/B/C results with play signals already tagged per post (Step 3)
- Scanner C Trending Topics section
- Assignment sheet from Phase 0 (available plays + bank entries)

**Play assignment rules:**
- **Use play signals from Step 3 as the starting point for reply play selection.** The scanner already tagged likely plays — Phase 1.5 confirms or overrides based on the full OP context. A tagged play is a candidate, not a commitment.
- T1 plays fill most slots. T2: 1-2 originals per session. T3: 1 test slot per session (doesn't count toward daily target).
- **Mandated plays get reserved slots regardless of tier:**
  - Emotional validation: 1 mandatory slot per session. Scan for it at Phase 1 start. If timing window is dead (no Tier DX post in last 60 min), hold the slot open; revisit at end of Phase 1.
  - Observational authority: the single original per session should rotate voice modes across sessions. Observational authority at least every other session. Check last daily file's voice mode before assigning.
- **Play-level experience bank assignment:** The play spec determines whether a bank entry is needed (`Experience bank: required / optional / no`). Only reserve entries for plays that require them. Don't assign bank entries to Emotional validation, Genuine/warm, or Narrative hijack.
- **Reserve bank entries before launching:** Before launching reply agents, reserve 1-2 entries from the available pool for the original play. Reply agents draw from remaining pool. Prevents contention.
- **Timing-window check:** For plays with a hard timing window (Emotional validation: 30-60 min; Narrative hijack: 2 hours; Accident/chaos: 2 hours), verify the window is viable before assigning. If not, substitute a non-timed play.
- **Play rotation:** Check last 3 daily files for play history. No play repeats within 3 sessions (except T1 reply plays which are structural). Track which plays ran in the HTML comment at bottom of each daily file.
- **Feedback signals from Phase 0a:** Apply deprioritizations and promotions before finalizing assignment sheet.

**Each play assignment includes:**
- Play name + tier + **format** (statement or long-form tweet, from play spec)
- **Target impression type:** before selecting a play, name which impression you're optimizing for this slot (bookmarks, reply-backs, profile-clicks, views, shares). Then confirm the play's success metric matches. This is a design-first constraint: pick the impression target, then pick the play that farms it.
- **Play spec excerpt** — extract the full entry for this play from `x/plays.md` and copy it directly into the assignment. Do NOT pass the full plays.md to draft agents. Draft agents receive only the excerpt for their assigned play.
- **Inspiration bank examples** — pull 2-3 entries from `x/inspiration-bank.md` for the assigned play. Draft agents study the dimensions (case, person, structure, rhythm) before writing. If the bank section is empty for this play, skip.
- **Device:** one rhetorical device selected for this draft (antanaclasis, anadiplosis, antithesis, paradox, litotes, twisted idiom, nominalization — see voice.md device selection table). Selected here, at assignment time, before drafting.
- Bank entry (if required by play spec)
- OP context (for reply plays): full OP text, engagement stats, URL

**Format-aware assignment:** Each session produces **1 long-form tweet original** (280-1400 chars, MUST trigger Show More) + replies. No statement-format originals unless explicitly requested. The single original carries the session's thesis. All creative energy on originals goes into making that one post dense and sharp.

### Phase 2: Draft posts (N parallel slots, 2 sequential agents per slot, staggered launch)

The main agent creates assignments as Phase 1 results arrive. Each draft slot now spawns **two sequential agents**: a draft agent, then an adversarial eval agent.

**Reply assignments (launch as scanners return):**
1. From Scanner A/B/C results: pick the top 7-8 reply targets (best scores, freshest, <3 hours old). That's 2 extra over the 5-7 target, buffer for Phase 3 drops.
2. For each reply, write one sentence stating what the OP is actually saying (point + emotion + subject). This drives play selection — do NOT call a play before understanding the OP's subject.
3. Select the play from plays.md that best serves the response. Assign unique play + unique bank entry per reply (no duplicates). Ensure play variety across the batch.
4. Launch reply slots immediately. Don't wait for Miner/Off-platform.

**Original assignments (launch when Miner + Off-platform return):**
5. From Miner + Off-platform results: pick 1-2 original post ideas (1 target + 1 backup in case it fails evals).
6. Assign from reserved bank entries + plays from Phase 1.5 assignment sheet. The original must be long-form tweet format.
7. Assign the weekly thesis angle to the original.
8. Launch original slot.

**Overproduce by ~20%.** Some drafts will fail batch evals. Spawning extras means Phase 3b can drop the weakest without going under target.

**Per-slot two-agent sequence:**

**Agent 1 — Draft agent** receives:
- Play spec excerpt (assigned play only, extracted from `x/plays.md` by main agent at Phase 1.5 — do NOT read plays.md yourself)
- Bank entry (if `Experience bank: required` in play spec)
- For replies: OP's full post text (if thread, full thread), **OP's post URL (mandatory)**, engagement stats
- For originals: mined line or off-platform fuel, **source URL (mandatory)**
- Banger principles (Step 5b), voice.md + quality-gates.md + x/voice.md context

Draft agent drafts to the play's generation brief. Runs only the evals listed in the play spec (from `x/common-evals.md`). Rewrites if any play-specific eval fails. Returns a passing draft. **Does not run batch evals (B1-B6) — those are main agent only.**

**Agent 2 — Adversarial eval agent** receives:
- The draft from Agent 1
- The play's eval subset from `x/common-evals.md`
- The play spec

**Critical: Agent 2 has no access to Agent 1's reasoning.** It only sees the draft and the eval list. Instruction: "For each eval, start from the assumption this draft FAILS. Find the specific failure mode. Only mark PASS if you genuinely cannot find it after actively looking." This is the opposite of confirmatory review.

If any eval fails → Agent 1 rewrites once, Agent 2 re-evals. **Max 1 rewrite.** If still failing after 1 rewrite, continue — include the draft anyway, do not silently drop. Agent 2 returns it with `Needs human review: yes` and notes the specific failing eval.

Agent 2 returns for each slot:
```
Draft: [text]
Eval score: Clean / Borderline / Tight
Predicted: ★★★☆☆
Drivers: [2-3 words]
Needs human review: yes/no (if yes, note which eval failed)
```
No full eval annotation block. The eval score communicates what needs to — if it's Borderline or Tight, {{YOUR_NAME}} knows to look closely.

**Mandatory pre-draft step for replies (Agent 1):** Before writing a single word:
1. Write one sentence stating what OP is actually saying (point + emotion + subject). This drives the draft.

The reply must directly engage with that sentence. Replies are simple thoughts about what was said. No rhetorical device selection, no arc planning. React naturally.

**Topic rule:** The device applies to the OP's subject, not {{YOUR_NAME}}'s product domain. Do not route observations through privacy, {{PRODUCT}}, ZK, or AI unless the OP's post is explicitly about those things. The observation stands on its own.

**Short-reply language rule (Agent 1, mandatory):** In 1-3 sentence replies, do not invent terms. Use language people already say. If the phrase works only because the writer understands it, rewrite.

**Emotional match rule for replies (Agent 1):** The reply must carry some of OP's emotional energy. If OP is excited, the reply can't sound like a calm postmortem. If OP is in disbelief, the reply needs some disbelief in it.

**Kill test (Agent 1, mandatory before finalizing any reply):** "Would this reply work if OP had posted something completely different?" If yes, rewrite.

**Replies are simple thoughts (Agent 1):** No sentence construction rules for replies. No arc, no hook, no landing. Express the thought about what was said and stop. Sentence construction rules (mechanism naming, contrast framing, closer formulation) apply to originals only.

The *OP is saying:* sentence appears in the output above the draft. Not decoration — any reviewer can check whether the draft actually engages with it.

Slots run in parallel. No coordination between slots.

### Phase 3: Batch eval + assembly (~3 min)

Phase 2 now handles per-draft eval verification (adversarial eval agent per slot). Phase 3 is batch evals + assembly only.

**Step 3a: Collect all drafts from Phase 2 slots.**

All slots return: draft text, eval score (Clean/Borderline/Tight), predicted ★, drivers (2-3 words), and `Needs human review` flag. No full eval annotation blocks. Main agent collects all of these.

**Step 3b: Sequential batch evals (main agent only)**

Batch evals require seeing all drafts together. Run sequentially after all Phase 2 slots return:

1. Run **B1-B6** across the full set (ending variety, cadence variety, no batch arc, register variety, ideological portability, play variety).
2. **If batch evals fail:** identify which specific drafts are offending, rewrite **one at a time**, re-run batch evals after each rewrite. Prevents two independent rewrites from colliding on the same batch constraint.
3. Rewrite priority: drop the weakest-predicted draft first (use predicted score to decide).
4. Repeat until all batch evals pass.

**Step 3c: Assembly**

5. Assemble the daily file. **Format (per draft block):**

```
**[Reply to @handle / Original]** · Play: [Name] ([Tier])
Eval: [Clean/Borderline/Tight] · Predicted: ★★★☆☆
Drivers: [e.g., T1 play, mid-tier OP (33K views), gap clean, thesis aligned]

OP: "[full text of the post being replied to — originals omit this field entirely]"

---
[draft text]
---

Gap: [annotation — what's left unresolved and why reader would click profile]
[For plays where Gap: forbidden, write: "n/a — [play name] uses completeness"]
Bank entry used: [entry name or "none"]
```

**End of daily file — feedback section (always include, always blank):**
```
## Feedback
[Write here after posting.]
[- "accusation play felt forced, skipped it"]
[- "emotional validation got 8K views — timing window is real"]
[- "promote Named Phenomenon — 2 posts above avg this week"]
[- "stop running Narrative Hijack until 2K followers"]
```

6. **Mandatory links:** Every reply block must include the OP's x.com URL. Every original must include a source URL (off-platform link or internal file path). No draft enters the daily file without a link.
7. Write to `cache/xgrow/[YYYY-MM-DD].md`
8. Close all browser sessions.

**HTML comment at bottom (rotation tracking, read by skill on next run):**
```
<!-- ROTATION TRACKING -->
<!-- Plays: Accusation T1 (R1), Emotional Validation T1 (R5), Direct Engagement T1 (R2) -->
<!-- Thesis: #1, angle #3 -->
<!-- Promotions: none this session -->
```

### Estimated time savings

| Step | Sequential | Old parallel | New parallel |
|---|---|---|---|
| Phase 0: Setup (∥ Phase 1) | 2 min | 2 min | 0 min (overlaps with Phase 1) |
| Phase 1: Scanning + mining + off-platform | 15 min | 5-7 min (3 agents) | ~3 min (5 agents, scanner split 3-way) |
| Phase 2: Drafting + per-draft evals (8-10 posts) | 15-20 min | 3-5 min | 3-5 min (staggered start saves ~1 min) |
| Phase 3: Eval verification + batch evals + assembly | 3 min | 3 min | ~1 min (adversarial parallel eval agents, no main-agent re-run, sequential batch only) |
| **Total** | **35-40 min** | **13-17 min** | **~6-8 min** |

## Browser Execution Pattern (agent-browser)

Same approach as Slack skill: agent-browser loads saved auth from state file. Navigate to `x.com`, interact with the page to find posts and extract content.

### Step 1: Navigate to X

Use `agent-browser --session x-scan --state ~/.agent-browser/x-auth.json open` to `https://x.com`. agent-browser loads auth from `~/.agent-browser/x-auth.json`. Verify login by checking for the home feed.

### Step 2: Search for target content

Use X's search to find recent posts from target accounts and topics. Search queries rotate through, **AI-first**:
- **Daily 8 (scan first, every day):** `from:ropirito OR from:ethermage OR from:MurrLincoln OR from:v_vashishta OR from:somewheresy OR from:_inesmontani OR from:marek_rosa OR from:CodeHagen`
- **Tier 1 bench (rotate in when daily 8 haven't posted):** `from:bencera OR from:chhddavid OR from:tessalau OR from:FelixCraftAI OR from:lishali88 OR from:0xROAS OR from:Unibase_AI`
- **Tier 2 handles (30K-100K, 3-4x/week):** `from:{{HANDLE_1}} OR from:{{HANDLE_2}} OR from:{{HANDLE_3}}`
- **Tier 3 handles (100K-300K, weekly only):** `from:Thom_Wolf OR from:lennysan OR from:alexalbert__ OR from:chipro OR from:shawmakesmagic OR from:Austin_Federa OR from:ljin18 OR from:mckaywrigley OR from:LinusEkenstam OR from:heyBarsee OR from:simon_willison OR from:OfficialLoganK OR from:shreyas`
- **Topic keywords (AI-first):** "Claude Code", "building with AI", "AI agents", "vibe coding", "shipping fast", "AI workflow", "coding with AI", "AI productivity"
- **Topic keywords (crypto/fintech):** "stablecoins", "crypto product", "privacy blockchain", "fintech UX", "AI agents crypto", "onchain privacy"
- **Trending topics** in crypto, privacy, AI, startups, product
- **Narrative hijack scan:** `"crypto is" OR "crypto scam" OR "waste of time crypto" OR "don't do crypto"` and similar outsider attacks trending with 1K+ likes. Check Explore tab for anti-crypto/anti-privacy narratives from mainstream tech accounts. See Step 7h.

Use `agent-browser --session x-scan snapshot` to read the page content. Use `agent-browser --session x-scan click` and `agent-browser --session x-scan fill` to interact with search.

### Step 2b: Scan off-platform sources for content fuel

See `projects/growsocial/planning/content-prep.md` → Off-platform section. Runs as a parallel agent in Phase 1.

### Step 3: Extract post data

Return a compact table. No prose, no raw browser output.

| Handle | Followers | Views | Likes | Replies | Age | Play signal | URL | Snippet (50 words max) |
|---|---|---|---|---|---|---|---|---|

**Play signal rules:**
- Explicit question or stated pain ("how do I...", "curious if anyone...") → **The fix**
- Strong take with a structural layer missing → **The reframe**
- Large account (50K+ followers), viral take, {{YOUR_NAME}}'s identity directly contradicts the framing → **Narrative hijack**
- Tier DX account, announcement/milestone/deprecation post → **Emotional validation** (check timing window)
- Announcement/launch from 200K+ account, thin reply section → **Genuine/warm**
- Anything else → **Direct engagement** (baseline)
- Multiple signals possible — list all that apply. Phase 1.5 makes the final call.

### Step 4: Score and rank

Prioritize posts where a reply will get maximum visibility:
1. **Recency:** **Hard cutoff: 3 hours old max.** Posts older than 3 hours get dropped. No exceptions. Reply sections die fast. If nothing fresh exists, say so and skip replies for this window.
2. **OP size sweet spot: 5K-50K views.** This is the #1 ranking factor from performance data. Conversion rate on mid-tier posts (20K-50K views) is 50-100x higher than mega-viral posts (200K+). The jswihart reply got 452 views on a 33K-view post. The karpathy reply got 41 views on a 2.3M-view post. Mega-viral posts bury you in hundreds of replies. Mid-tier posts keep you visible. **Deprioritize posts with 200K+ views unless you have an exceptionally sharp take.**
3. **Reply gap:** Post has few quality replies (easier to stand out). Under 100 replies is ideal. Over 300 is dead.
4. **Topic fit:** Matches {{YOUR_NAME}}'s content pillars. **AI/builder threads get 2x priority weight.** AI is 80% of the content strategy. Crypto/fintech threads are domain color, not the primary target.
5. **Engagement velocity:** High likes relative to time posted
6. **Announcement/launch post from 200K+ account.**

**Tiebreaker (when criteria 1-2 tie):** use reply gap (3), then velocity (5), then topic fit (4). If still tied, prefer the account with higher reply-back probability (see Reply-Back Targets table). Product announcements, tool launches, and link-card posts from large accounts get bookmarked, not debated. This creates thin reply sections (<50 replies) with massive view counts. Signal: high bookmark-to-reply ratio (>5:1). These are prime targets for genuine replies (style #4). Recency still matters but the 3-hour cutoff can stretch to 6 hours since reply sections stay thin longer on announcement posts. **Data point:** @addyosmani Google Workspace CLI announcement → 22:1 bookmark-to-reply ratio. {{YOUR_NAME}}'s genuine reply 2 hours later with 1-3 existing replies → 13.3K views, best performing post ever.

### Step 5: Reply vs Quote Tweet decision

**Default is reply.** Replies borrow the OP's audience. QT when: reply count >300 (yours drowns), your take stands alone without the original, or reply section is dead (>48 hours).

### Step 5b: Banger principles (load before drafting originals)

Read these before writing originals. These principles shape good original posts. **They do not apply to replies.** Replies are simple thoughts about what was said.

**1. The gap is everything.**
A post that delivers its insight completely is a good post and a bad growth post. The reader got what they needed. They leave. A post that *implies* depth without resolving it forces a profile click. "The hard part isn't X, it's Y" delivers and closes. "3 weeks into rebuilding our entire workflow around claude. the part nobody warns you about isn't the AI" opens a world the reader can only explore on your profile.

**2. Insider details are the credibility mechanism.**
Not "shipping is hard." That's a bumper sticker. "decimal precision bugs at 2am" or "third attempt at gas abstraction" or "proving pipeline is memory-bound and that one constraint shaped 3 months of decisions." The specificity implies a world behind the post. The stranger thinks "who is this person?"

**3. Tension that doesn't resolve.**
Tension works in originals because the reader needs to click your profile to resolve it. In replies, the OP's post IS the context. Don't engineer tension into a reply.

**4. One idea per post. The idea should be stealable.**
If someone can screenshot your post and send it to a friend with no context and the friend gets it, that's a banger. "data confirms. it doesn't discover." is stealable. "I think we should use more data but also talk to users" is not.

**5. The screenshot test.**
Before including an original draft, imagine you saw it from a stranger. Would you stop scrolling? Would you click their profile? If not, it's not ready. Kill it or rewrite.

**6. Experience bank + format = banger.**
The experience bank has 60+ entries. The format list has 12+ formats. A banger is almost always: one experience bank entry (the substance) shaped by one format (the hook), with a gap left open (the follow). Don't draft from scratch when you have both ingredients.

### Step 6: Draft replies and QTs

For each target post, draft content following the Reply Rules below. Tag each draft with its action (Reply or QT).

### Step 6b: Mine outputs for original post material

See `projects/growsocial/planning/content-prep.md` → Miner section. Runs as a parallel agent in Phase 1. Results available here for drafting.

### Step 6c: Weekly thesis check

Check the weekly thesis before assigning any **original** posts. Reply drafts can launch before this step completes. One idea per week, hit from 7 different angles across the week's posts (originals, replies, threads, narrative hijacks). The thesis lives in the daily file.

**Process:**
1. Read the most recent daily file in `cache/xgrow/`. Find the `## Weekly Thesis` section.
2. If it's Monday or no thesis exists, pick a new one. Source it from: `docs/strategy/`, `docs/narrative/`, or a strong claim from the experience bank. The thesis should be one sentence a stranger could disagree with.
3. Check the thesis tracker: which angles have already been used this week? Each daily file logs which angle was used.
4. At least 1 post per daily session must reinforce the weekly thesis from a new angle. Different angle means different format, different framing, or different context (reply vs original vs thread).

**7 angle types (use each once per week max):**
1. Direct claim (original post stating the thesis)
2. Reply that extends someone else's post toward the thesis
3. Personal story / builder log that demonstrates the thesis
4. Metric or data point that supports the thesis
5. Reframe of a counter-argument (someone saying the opposite)
6. Thread that explores the thesis in depth
7. Narrative hijack where the thesis is the twist

**Why:** People need to encounter your message ~7 times before it sticks. One post per idea = forgettable. One idea across 7 posts in a week = you own the topic. Topic ownership is what converts profile visitors to followers.

### Step 7: Draft original posts (content production process)

This is the full process for producing original content. Run it every time originals are needed.

**7a. Mine results (from Phase 1 Miner agent).**
Use results already returned by the Miner agent. Pull from those: specific numbers, contrarian claims, product tradeoffs, competitive observations. Do not re-scan docs/.

**7b. Experience bank + format = banger (MANDATORY).**
Every original must start from a specific experience bank entry (`projects/growsocial/planning/bank.md`). The entry gives the substance. A format from the format list below gives the hook. The gap left open gives the follow. This is the formula: pick an entry, pick a format, leave something unresolved. Don't draft from scratch. Don't draft from abstract reasoning. If the draft doesn't trace back to a real experience {{YOUR_NAME}} lived, it fails eval #16.

Also cross-reference mined lines from 7a: match each to a bank entry. A data point from research + a personal angle from the bank = a post that's both credible and personal. If no bank match exists, the data point can stand alone only if it has a specific number.

**7c. Call a play from the playbook.**
Take each (experience bank entry + play) pair and shape it. Don't default to the same play twice in a row. The full playbook lives in `projects/growsocial/x/plays.md` — read the play's generation brief directly. Each play maps to a voice mode and slot type. The Phase 1.5 assignment sheet already called the plays; this step executes the generation brief for each assigned play. Track which plays ran in the HTML comment at bottom of the daily file.

**7d. Filter through writeSocial rules.**
Density mandatory. Specific number or vivid metaphor. Tension first. No product names. Growth mode (stranger can screenshot it). Run the passive observer quality gate.

**7e. Assign posting windows.**
Space originals at least 1 hour apart. No hard spacing rule. If producing a multi-day batch, create a posting schedule table.

**7f-pre. Awe test (MANDATORY for originals, run before depth gate).**
Does this draft create awe? Would a stranger feel slightly expanded, not just informed? Check for at least one: scale shift, unexpected connection, behind-the-curtain detail, or contrarian clarity. If it's just a good point without awe, find the angle that makes the reader think "I never thought of it that way." See writeSocial.md "Awe as virality lever" for the full framework.

**Awe test does not apply to replies.** Replies are simple thoughts. They don't need to create awe. They need to engage with what was said.

**7f. Depth gate (play-level — not universal).**
The depth gate (unresolved gap, profile click, insider detail, tension not self-resolved) now lives in the play spec as evals P1, P2, P3. It runs only for plays where `Gap: required`. It does NOT run for plays where `Gap: forbidden` (Emotional validation, Genuine/warm, One-liner, and any other completeness play).

**In the daily file:** every gap-required draft must include a `Gap:` annotation. For completeness plays, write `Gap: n/a — [play name] uses completeness`. This is enforced in Phase 3c assembly.

**Kill pattern still applies for gap-required plays:** drafts that are complete thoughts. "The hard part isn't X, it's Y" delivers the insight and closes the loop. "killed 6 features this month" opens a loop that only the profile can close.

**7g. Anti-slop gate (MANDATORY, run on every draft alongside depth gate).**
`GrokSlopScoreRescorer` can suppress AI-generated content to 0.0 reach. Every draft must pass these checks before entering the file. If any check fails, rewrite.

1. **All LLM tells from voice.md apply.** Summary bows, corrective antithesis, vague intensifiers, hedged preambles, restatement closers, "and that's" bridges, parallel triads. voice.md is the canonical list. If it's killed there, it's killed here.
2. **Vary sentence length across drafts.** If 3+ drafts in the same daily file have similar word counts or cadence, rewrite the outliers. A batch of posts that all sound the same is the strongest slop signal.
3. **No ascending clause length.** Each item in a list getting progressively longer and more polished is a structural fingerprint. Keep lists uneven.
4. **No clean arc across a batch.** If the 5 originals feel like chapters in a sequence (setup → build → insight → pivot → lesson), scramble the order. Real posting is scattered, not narratively coherent.
5. **Break punctuation rules.** Perfect grammar across every draft reads as mechanical. Fragments, missing periods, lowercase, run-on thoughts. Match how {{YOUR_NAME}} actually texts (see voice.md chat rules).
6. **No identical endings.** If multiple drafts end on a punchy one-liner with the same rhythm, rewrite. Vary how posts close: some trail off, some end mid-thought, some end on a question.

**Diagnostic:** Read all drafts in the daily file back-to-back. If they feel like they came from the same author prompt, they'll flag as slop. They should feel like posts written at different times of day in different moods.

**7i. Eval gate (handled per-draft in Phase 2 by adversarial eval agent)**

Evals are now run per-draft in Phase 2 by the adversarial eval agent. The single source of truth is `projects/growsocial/x/common-evals.md` (universal evals U1-U4 + play-specific evals P1-P11) and each play's eval subset in `projects/growsocial/x/plays.md`. Batch evals (B1-B6) run in Phase 3b.

**The eval agent runs the evals listed in the play spec — not all evals universally.** This is the core architectural change. A one-liner play (Gap: forbidden) does not run P1 (gap survives). An emotional validation play does not run P11 (experience bank sourced). Each play specifies exactly which evals apply.

**Critical: voice match (U2) is not a style checklist.** The test is: would {{YOUR_NAME}} text this to a friend? Citing reports = fail. Tool names strangers wouldn't know = fail. Clean narrative arc building to a revelation = fail. Read the draft out loud. If it sounds written, it fails.

**Original post rules:**
- **Format-aware drafting.** Check the play's `Format:` field before writing. Statement-format plays: under 280 chars, one sentence, no Show More. Long-form tweet plays: 280-1400 chars, story structure (context → results → principle), MUST trigger Show More.
- **Show More placement (long-form tweet format only).** After drafting, check where the ~280 char truncation lands. Place the most valuable line immediately below the fold. This triggers a 20x algorithmic boost (ControlAiRescorer "Show More" signal). The first 280 chars create curiosity. The line below Show More rewards the click. Restructure if the best line sits above the fold.
- **Open with tension, not explanation.** First words must create a gap the reader needs to close.
- **No length limit. Density is mandatory.** Every sentence must add tension, a data point, or a curiosity gap. Cut anything that resolves without adding.
- **Every original should have at least one specific number.** "18 times", "15 minutes", "$1.27B", "90 days." Numbers create credibility and stop the scroll.
- **No system descriptions.** Nobody cares about your system. They care about the outcome or the claim.
- **Group chat posts don't count toward daily target.** Cap at 2/day.
- **One original per session, always long-form tweet.** No statement-format originals. The single original is the anchor. Make it count.

**Original post plays** — full playbook with generation briefs, tiers, eval subsets, and all spec fields lives in `projects/growsocial/x/plays.md`. Read that file. Do not use this section as the play reference — it has been migrated.

*Migrated to plays.md as of 2026-03-08. Updated 2026-03-21: 39 plays total (21 original long-form tweet, 7 original statement, 1 distribution, 8 reply, 2 long-form). Format field added to all original plays.*

### Step 7h: Narrative hijack scan (run every scan session)

Large accounts regularly drop provocative takes that attack crypto, privacy, or AI-native building. When a post offends or challenges a community {{YOUR_NAME}} belongs to, reposting the hottest line as an original — with a twist from {{YOUR_NAME}}'s perspective — borrows the trending topic's distribution without competing in the reply section.

**Data point:** @steipete "don't waste time with crypto" (6.2K likes, 640 replies). {{YOUR_NAME}} reposted the line verbatim → 250 views (highest ever), 4% profile visit rate (10x normal). The contradiction between the post and {{YOUR_NAME}}'s crypto-builder bio drove profile clicks.

**When to trigger:**
1. A large account (50K+ followers) posts a take that attacks crypto, privacy, builders, or AI-native work
2. The post is gaining momentum: 1K+ likes within 2 hours, 100+ replies (controversy, not just agreement)
3. {{YOUR_NAME}}'s bio or identity directly contradicts the take (this is what drives profile clicks — the reader needs to resolve the tension)

**How to draft:**
1. Extract the single hottest line from the trending post (under 10 words ideal)
2. Add a short twist from {{YOUR_NAME}}'s perspective. Options ranked by profile-click potential:
   - **Identity contradiction:** Post the line and let the bio do the work. "don't waste time with crypto" from a crypto builder. Maximum curiosity gap.
   - **The insider's agreement:** Agree with the outsider's critique but from the inside. "he's right. most crypto products are a waste of time. the 3 that aren't are the whole point." Implies insider knowledge of which 3.
   - **The reframe:** Take the attack and redirect it. "don't waste time with crypto. unless you've seen what a public ledger does to someone's financial privacy." Turns the attack into {{YOUR_NAME}}'s thesis.
3. Post as an **original** (not QT, not reply). Originals get full 1.0x algorithm score. QTs split attention. Replies get buried in 640+ reply sections.

**Rules:**
- Max 1 per day. 2-3 per week max. This is a spike tool, not a daily strategy. Overuse looks like engagement farming.
- The twist must hint at {{YOUR_NAME}}'s identity without naming products or chains. The bio resolves the gap.
- Must post within 2 hours of the trending post (topic association decays fast).
- Don't hijack takes from people in {{YOUR_NAME}}'s network or community. Only outsider attacks. Repurposing a take from @shawmakesmagic or @zooko would be weird. Repurposing from @steipete or a mainstream tech account works because they're outside the crypto builder world.
- Track what you hijacked in the Angles Used table so you don't repeat the same controversy twice.

**In the daily file:** Flag hijack candidates under a `## Narrative Hijack Candidates` section with the source post URL, engagement stats, and the drafted twist. {{YOUR_NAME}} decides whether to post.

### Step 7j: Weekly article (invoke xarticle)

X is pushing long-form articles algorithmically. Articles serve a different function than short posts: they build authority and get bookmarked, but they don't get reposted much. The article is the reason someone follows after seeing your short post. A stranger clicks your profile and finds depth.

**Cadence: twice per week, mandatory.** Check `cache/xarticle/` for the most recent article date. If the last article is 3+ days old, this session must include an article. If <3 days, skip.

**How to invoke:** xgrow does NOT draft the article itself. Instead, it invokes the `/xarticle` skill as a parallel agent during Phase 2. Pass the xarticle agent:
- The current weekly thesis (from Phase 0 content-prep)
- Scanner C's full Article Intelligence section (articles found, meta/cultural moments, narrative momentum)
- Scanner C's trending topics (from Phase 1)
- Any strong miner/off-platform fuel that has article-depth potential
The xarticle skill handles all drafting, structure, and evals. Its output lands in `cache/xarticle/`.

**In the daily file:** Always add a `## Article Intel` section with the best items from Scanner C's Article Intelligence output (even on non-article weeks). On article weeks, also add a `## Article` section linking to the xarticle output file with the article's thesis and a one-line self-QT angle.

**Source:** Reverse engineering @MarkKnd article (Feb 16, 2026). 183K views on 30K followers (6x). 2.7K bookmarks vs 61 reposts. Articles get saved, not shared. Short posts get shared, not saved. Use both.

### Step 7k: Self-QT distribution (post-publish, not part of daily drafting)

After publishing originals, monitor performance at the 4-6 hour mark. If any original is significantly below its predicted score, draft a self-QT using the Self-QT (second life) play from `x/plays.md`.

**Why this works:** When you quote your own post, the views on the quote count toward the original's total impression count. A self-QT with a different angle gives the algorithm a second distribution window and attracts a different reader segment.

**Process:**
1. Check original's views at hour 4-6. If below predicted score, trigger.
2. Draft a standalone one-liner that reframes the original from a completely different angle.
3. Post as a quote tweet of the original. Not "icymi", not a summary. A new entry point.
4. Max 1 self-QT per day. This is a rescue tool, not a routine.

**Data point (Chua):** article at 5K views after 6 hours → self-QT with one-liner → 400K impressions on the quote, all counting toward the original's total.

**In the daily file:** Include a `## Self-QT Candidates` section listing which originals to monitor and what alternate angle to use if they underperform. {{YOUR_NAME}} decides post-publish.

### Step 8: Weekly thread draft

Every Monday (or when explicitly asked), draft a thread. Hook-first structure. On non-Monday runs, skip this step unless explicitly requested.

## Content Pillars

Weighted for maximum growth velocity. Load from `projects/growsocial/x/strategy.md` for full breakdown, example posts, and voice rules.

| Pillar | Weight | What it covers |
|---|---|---|
| AI-native building | 80% | How you use AI to ship. Claude Code, agents, prototyping, scoping, debugging, competitive analysis, morning workflows, team leverage. Always framed as outcomes ("shipped in 2 days") not process ("synthesized research"). Crypto/fintech shows up as domain color inside AI posts. Practitioner POV only. |
| Personal | 10% | Lifting, fatherhood, communication craft. Discipline lens, tension, what's hard. Not fitness content, not cute stories. |
| Product/shipping takes | 10% | Decisions, what broke, what you learned from users. Domain-agnostic, builder language. Any founder relates. |

**Voice identity:** Builder, not PM. Never say: PRD, stakeholder, alignment, synthesize, iterate, leverage, cross-functional, OKR, sprint. See vocabulary swap table in `projects/growsocial/x/strategy.md`.

**Identity test:** Before posting, ask: "Would a builder say this, or would a product manager say this?" If PM, rewrite.

## Social Writing Style

**Load `.claude/skills/utils/voice.md` then `projects/growsocial/planning/quality-gates.md` then `projects/growsocial/x/voice.md` before drafting any X content.** All voice.md rules apply. Quality gates add shared pre-generation checks. X voice.md is additive: shorter sentences, fragments, first person singular, hook formulas, and post formats.

## Reply Rules

### Reply plays (ranked by performance)

Full play specs with generation briefs, eval subsets, and timing rules live in `projects/growsocial/x/plays.md` → Reply plays section. The hierarchy below is the priority order for play selection:

1. **Direct engagement (T1).** Respond to what OP actually said — counter, sharpen, or extend. Stay on their subject.
2. **Contrarian/provocative (T1).** Challenge OP's point or assumption directly. Tension gets views.
3. **Specific personal experience (T1).** Specific workflow anchored to OP's subject. Not generic credentials.
4. **Genuine/warm (T1).** Real reaction only when the feeling writes itself. Cap 1-2/day. Data: @addyosmani CLI reply → 13.3K views, best performing post ever.
5. **Emotional validation (T1, mandatory 1/day).** ≤12 words. Universal shared feeling in Tier DX thread. 30-60 min hard window. Data: "always a bitter sweet moment..." → 40,500 views, top reply.
6. **Witty/short (T2).** One sentence riff inside OP's frame. Only when the joke writes itself.
7. **KILL: Complimentary / agreeing.** No new angle = skip the post.
8. **KILL: Generic agreement.** Restating OP in different words. Dead on arrival.

### What makes a great reply

**Replies are simple thoughts expressed around what the OP said.** They don't need a hook, a landing, or an arc. They're not mini-posts. Say the thought and stop.

- **React to what was said.** The reply is about the OP's subject, not your positioning. If you're engineering a gap or a profile click, you've left "reply" territory and entered "original post disguised as a reply."
- **One thought is enough.** Don't stack observations. Don't build to a point. Just say the thing.
- **Extension > agreement.** Take the OP's point somewhere they didn't go. But keep it casual, not strategic.
- **First-person OR observation.** "I" is not required. "sovereignty without privacy is a contradiction" (452 views, best performer) has no "I." The reply must come from experience, but doesn't have to announce it.
- **Starts with substance, not agreement.** Never start with "Great take!" or "This." or "100%."
- **Never compliment the OP.** Zero likes on every complimentary reply in the dataset. If your draft starts with appreciation, skip the post.
- **No gap engineering.** Don't leave breadcrumbs to your profile. Don't withhold information for curiosity. Just say the thought completely.

### Anti-slop gate (applies to replies too)
Every reply must pass the anti-slop gate (Step 7g). Confirm: doesn't share structure/cadence/length with other drafts in the batch. Depth gate (gap, profile click, insider detail) does NOT apply to replies. Replies are simple thoughts, not growth-engineered posts. Gap annotation on replies: `Gap: n/a — replies are simple thoughts`.

### What to never do in replies
- **Reply to truncated posts without reading the full text.** Always click "Show more" before drafting. The thesis is usually in the last few paragraphs, not the first. Replying to the setup instead of the point is worse than not replying at all.
- "Great thread!" / "This is so true" / any empty validation
- Mention product or chain names. Triggers crypto spam filter (`HighCryptospamScoreConvoDownrankAbusiveQualityRule`) which collapses your reply.
- Tag people for attention. Earn replies, don't beg.
- Hashtags in replies (looks spammy)
- Write more than the original tweet
- Start with "I agree" (boring, invisible)
- Include external links (kills on-platform engagement signals)

## Thread Rules

Threads are the primary format for bookmark-worthy and repost-worthy content. 1 repost this week. Reposts put your name in new feeds → profile visits.

**Structure:**
- 3-5 tweets. Every tweet stands alone. No "1/" on the hook.
- Hook tweet follows all original post rules: tension first, no length limit but density is mandatory.
- Each subsequent tweet: one fact, one tradeoff, or one data point. No setup tweets, no "let me explain" tweets. Every tweet screenshottable alone.
- Target: 1 thread per week minimum.

**Thread hooks that work:**
- **The declarative + controversial:** "If you use it right, X is the most [claim]." Reader disagrees, keeps reading.
- **The story start:** "Someone asked me [question] yesterday. My answer turned into this thread." Natural, non-promotional.
- **The research reveal:** "I [action] and found [surprising thing]." The gap between what you did and what you found is the hook.
- **The partial reveal:** "There's a pattern I keep seeing in [topic]." The reader needs the pattern.

**Source material for threads:** `docs/strategy/`, `docs/market-research/`, `docs/product/`, product glossary. Turn internal knowledge into structured takes a stranger can reference.

## Target Accounts

Load the full list from `projects/growsocial/x/strategy.md`. Sweet spot is **5K-30K followers** where replies stay visible.

### Daily 8 (reply every day — highest ROI)

@ropirito (22.7K), @ethermage (29.3K), @MurrLincoln (12.8K), @v_vashishta (28.4K), @somewheresy (27.5K), @_inesmontani (20.4K), @marek_rosa (12.2K), @CodeHagen (9.4K)

### Tier 1 bench (rotate in when daily 8 haven't posted)

@bencera (5.8K), @chhddavid (6.9K), @tessalau (9.2K), @FelixCraftAI (11.4K), @lishali88 (17.5K), @0xROAS (25.7K), @Unibase_AI (27.9K)

### Tier 2: 3-4x/week (30K-100K — only with <100 replies)

@{{HANDLE_1}} ({{FOLLOWERS}}), 

### Tier 3: Weekly (100K-300K — only with genuinely sharp take)

@Thom_Wolf (104K), @lennysan (120K), @alexalbert__ (122K), @chipro (126K), @shawmakesmagic (163.7K), @Austin_Federa (177.4K), @ljin18 (180K), @mckaywrigley (226K), @LinusEkenstam (228K), @heyBarsee (273K), @simon_willison (275K), @OfficialLoganK (279.9K), @shreyas (313K)

### Tier 4: Skip (300K+ — reply drowns)

@mert (325.2K), @emollick (327.8K), @mattshumer_ (351.1K), @petergyang (420K), @guillermo_rauch (450K), @rowancheung (573K), @swyx (627K), @dharmesh (680K), @tobi (700K), @levelsio (825K), @nikitabier (827K), @garrytan (1.1M), @karpathy (1.2M)
Only if you catch it in the first 5 minutes with <20 replies. **Exception: genuine replies (style #4) are allowed on any 200K+ account regardless of timing.** Announcement/tool/launch posts from large accounts often have thin reply sections (high bookmarks, low replies) because link cards suppress reply action visually. These are high-visibility, low-competition windows. See Step 4 signal #6.

### Tier DX: Emotional validation targets (∞ size — style #5 only)

@addyosmani (400K+ DX/frontend), @kentcdodds (250K+ testing/React), @ThePrimeagen (250K+ performance/Rust), @wesbos (260K+ fullstack/DX), @leeerob (100K+ Next.js/Vercel DX), @cassidoo (95K+ DX/open source), @rachelnabors (85K+ web platform), @sarah_edo (80K+ Vue/DX), @karpathy (1.2M AI/learning — milestone/curiosity moments), @simonw (275K open source — deprecation/maintainer moments), @emollick (327K AI-in-education — community AI milestones), @mckaywrigley (226K AI builder — ships things, community moments), @zooko (200K privacy/cypherpunk — privacy-as-human-right moments, open source)

Follower count is irrelevant here — the **audience profile** is what matters. These accounts have large DX/developer/open source communities who amplify emotional moments. Only engage via **Style #5 Emotional validation**. Do not use standard reply rules (gap engineering, profile click optimization) on this tier. Scan for trigger threads during Phase 1 Scanner A. Window: within 30-60 min of post. One per daily run.

### Tier 5: Ecosystem + privacy (relationship, not growth)

@{{CHAIN}}HQ, @{{COMPANY}}HQ, @{{INVESTOR_FIRM}}crypto, @matthew_d_green, @zooko, @BostonZcash, @TheDesertLynx
Don't count toward daily reply goals. Engage for community reasons only.

## Reply-Back Targets (who actually replies back)

Prioritize accounts where reply-back probability is high. Reply-backs = 75x a like in the algorithm.

| Account | Tier | Reply-back likelihood | What triggers a reply-back |
|---|---|---|---|
| @ropirito | Daily 8 | HIGH | AI agents + crypto. Technical observations about agent infra |
| @ethermage | Daily 8 | HIGH | AI + crypto infra. Opinionated, defends positions |
| @MurrLincoln | Daily 8 | HIGH | AI + crypto builder. Engages on builder topics |
| @v_vashishta | Daily 8 | HIGH | Enterprise ML + fintech. Loves debate |
| @CodeHagen | Daily 8 | HIGH | Open-source AI agents. Active engager |
| @PawelHuryn | Tier 2 | HIGH | Builder observations about AI workflows. Posts constantly |
| @{{HANDLE_2}} | Tier 2 | HIGH | AI engineering. Technical depth triggers responses |
| @jessepollak | Tier 2 | MEDIUM | Product-level observations about onchain UX |
| @mckaywrigley | Tier 3 | MEDIUM | AI builder specifics. Ships with Claude, engages on workflow |
| @shawmakesmagic | Tier 3 | MEDIUM | Specific technical observations about agent infra. Now Tier 3 (163.7K) |
| @Austin_Federa | Tier 3 | MEDIUM | Opinionated. Defends positions. Now Tier 3 (177.4K) |

### Voice models

Sound like a builder showing receipts, not a pundit:

- **@levelsio** — Shows the work. "built X. broke Y. shipped Z anyway." Revenue numbers, real failures. The anti-pitch.
- **@nikitabier** — Specificity as weapon. One precise observation instead of a framework. Deadpan.
- **@swyx** — Connects dots across tools/ecosystem. From experience, not theory.
- **@mckaywrigley** — AI-native builder. Shows what he built with AI, not what AI can theoretically do.

**The rule:** Sound like a builder showing receipts. Never sound like an analyst summarizing the conversation.

### Daily pick logic
0. **Scan for 1 emotional validation (style #5) target.** Before any other scanning, check Tier DX accounts (@addyosmani, @kentcdodds, @ThePrimeagen, @wesbos, @leeerob, @cassidoo, @rachelnabors, @sarah_edo, @karpathy, @simonw, @emollick, @mckaywrigley, @zooko) for a trigger thread posted in the last 60 min (community project deprecated, maintainer moment, open source milestone, bitter sweet builder moment). If found, draft the emotional validation reply immediately — it has a hard 30-60 min window. If not found, this slot stays open; revisit at end of Phase 1.
1. **Scan daily 8 first.** Anyone post in the last 3 hours? Reply. These are the highest-ROI accounts — consistent engagement builds recognition.
2. **If daily 8 haven't posted, scan bench.** Rotate in @bencera, @chhddavid, @tessalau, @FelixCraftAI, @lishali88, @0xROAS, @Unibase_AI.
3. **Check for trending AI discourse.** Big AI story breaking? Post within 30 minutes.
4. **Check Tier 2 (30K-100K).** Only posts with <100 replies.
5. **Tier 3 (100K-300K) only with a genuinely sharp take.** If nothing sharp, skip.
6. **Tier 4 (300K+): skip unless first 5 minutes with <20 replies.** Proven by data: 41 views on a karpathy reply. **Exception: genuine replies (style #4) on 200K+ accounts are always allowed.** Scan for announcement/tool/launch posts with high bookmarks and <50 replies. These have massive audiences with thin reply sections. Data point: genuine reply to @addyosmani (mass following) → 13.3K views, 25 likes, 10 bookmarks, reply-back from OP.
7. **AI vs crypto ratio: 80% AI/builder, 20% crypto/fintech domain color.**
8. Diversify across accounts. Don't reply to the same person 3+ times in a day.
9. **One fast reply > five late ones.** Fresh post (<1 hour) from daily 8 beats older post from bigger account.
10. **Before drafting any reply, check: is this a reframe or a compliment?** If compliment, skip.

## Output Format

The daily file is {{YOUR_NAME}}'s posting queue. Only what he needs to post. All internal work (eval annotations, miner lines, off-platform fuel) stays in the skill's working memory and goes into an HTML comment at the bottom.

```markdown
# X — [Day, Month DD]

## Replies

**Reply to @[handle]** · Play: [Name] ([Tier])
Eval: [Clean/Borderline/Tight] · Predicted: ★★★☆☆
Drivers: [e.g., T1 play, mid-tier OP (33K views), gap clean, thesis aligned]

OP: "[full text of the post being replied to]"

[post URL]

---
> [Reply text]
---

Gap: [what's left unresolved — why reader clicks profile]
Bank entry used: [entry name or "none"]

---

## Originals

**Original** · Play: [Name] ([Tier]) · Format: [statement/long-form tweet]
Eval: [Clean/Borderline/Tight] · Predicted: ★★★☆☆
Drivers: [e.g., T1 play, gap clean, thesis aligned angle #3]

[source URL or file path]

---
> [Post text]
---

Gap: [annotation or "n/a — [play name] uses completeness"]
Bank entry used: [entry name]

---

## Article Draft (every 1-2 weeks, Mondays preferred)

**Article** · Play: Article (T2)
Eval: [Clean/Borderline/Tight] · Predicted: ★★★☆☆
Drivers: [thesis match, bookmark potential]

---
> [Full article text with section headers]
---

Gap: n/a — article uses completeness
Bank entry used: [entry name]

---

## Thread (weekly, Mondays)

**Thread** · Play: Weekly thread (T2)
Eval: [Clean/Borderline/Tight] · Predicted: ★★★☆☆
Drivers: [...]

---
> [Hook tweet]
>
> [Tweet 2]
>
> [Tweet 3]
---

Gap: [hook gap annotation]
Bank entry used: [entry name]

---

## Self-QT Candidates
[For each original: predicted score, alternate angle for self-QT if underperforming at hour 4-6]
[- Original #1 "..." → if <X views at hour 5, QT with: "..."]
[- Original #2 "..." → if <X views at hour 5, QT with: "..."]

## Feedback
[Write here after posting. Read by skill at next session start.]
[- "accusation play felt forced, skipped it"]
[- "emotional validation got 8K views — timing window is real"]
[- "promote [play name] — 2 posts above avg this week"]
[- "stop running [play name] until [condition]"]

<!-- ROTATION TRACKING (read by skill on next run, not by {{YOUR_NAME}}) -->
<!-- Plays: Accusation T1 (R1), Emotional Validation T1 (R5), Direct Engagement T1 (R2) -->
<!-- Thesis: #1, angle #3 -->
<!-- Promotions: none this session -->
```

## Reverse Engineering

See `projects/growsocial/planning/get-new-formats.md`. Cadence TBD.

## Weekly Tracker Update

Every 7 days (or when asked), update `projects/growsocial/x/performance.md`:
- Current follower count (ask {{YOUR_NAME}} or check via agent-browser)
- Top performing posts from the week
- What's working (reply topics, post types, times)
- What's not working
- Adjustments to strategy

## Algorithm Rules

Full analysis in `projects/growsocial/x/algorithm.md`. These rules are derived from X's open-source algorithm.

- **Prioritize conversation starters over one-shots.** `ReplyEngagedByAuthorParam` is a dedicated model weight. Replies that get the OP to reply back are algorithmically the strongest signal. Draft replies as questions or contrarian reframes that invite response.
- **Reply to every comment on your own posts, for hours not minutes.** Replying to comments on your originals is the highest-weight algorithmic signal available. Each reply generates conversation that compounds the post's reach. Not replying signals inactivity to the algorithm and actively reduces distribution. Don't stop after the first hour — keep engaging for as long as replies keep coming. Even an emoji counts. Data point (Chua): 10+ hours replying to comments on a single viral post.
- **Originals carry full score, replies get 0.75x.** Minimum 1 original per day, 2 if sharp hooks are available. Don't force originals — one good one beats two weak ones. Replies to mid-tier OPs are currently higher-ROI than originals at this follower count.
- **Monitor follower ratio.** Flag if following/follower ratio exceeds 0.6 with 500+ following. Exponential penalty on account reputation.
- **No external links in posts.** Links don't generate dwell time or on-platform engagement signals. If {{YOUR_NAME}} needs to link, put it in a reply to the original post.
- **Voice calibration is algorithmic protection.** Grok slop detection (`GrokSlopScoreRescorer`) penalizes AI-generated content. Every draft must sound like {{YOUR_NAME}} wrote it, not Claude.

## Rules

- **One file per day.** If `cache/xgrow/[YYYY-MM-DD].md` already exists, update it in place. Don't create a new file. Add an `## Updated [HH:MM]` section at the top with what changed so {{YOUR_NAME}} can see the diff at a glance.
- **Never post on {{YOUR_NAME}}'s behalf.** Output is drafts only. Always. No exceptions.
- **No product or chain names in drafts.** Don't mention {{PRODUCT}}, {{CHAIN}}, or {{COMPANY}} in any reply, original, or thread unless {{YOUR_NAME}} explicitly asks. Say "privacy wallet", "privacy chain", "the wallet layer" instead. The exception is replies to ecosystem accounts (@{{CHAIN}}HQ, @{{COMPANY}}HQ, @1{{FOUNDER_CEO}}Wu) where the context already names them. Product names in replies also risk triggering the crypto spam filter.
- **Technical accuracy is mandatory.** Every claim about how crypto, ZK, privacy, or AI works must be correct. Cross-check against `CLAUDE.md` glossary for {{PRODUCT}}-specific tech (delegated proving, remote proving, local proving, record scanning, etc.). Don't invent metrics, stats, or numbers. If you don't know a number, use soft language ("dozens", "thousands") or omit it.
- **Close the browser session after scan.** Run `agent-browser --session <name> close` when done. Each session is isolated, no conflicts between sessions.
- **Volume matters.** Target 6-8 posts/day (5-7 replies + 1 long-form original + 2 articles/week via xarticle). But never post a reply that doesn't create a reason to click your profile.
- **Track what works.** Each daily should reference yesterday's performance if data is available.
- **Adapt the strategy.** If a pillar isn't getting traction after 2 weeks, adjust weights in strategy.md.
- **Stay authentic.** These are {{YOUR_NAME}}'s posts. They should sound like him, not like a growth hacker.
- **No repeat angles within 5 days.** Before drafting, read the last 5 files in `cache/xgrow/` and check their "Angles Used Today" tables. If a proof point appeared in the last 5 days (e.g. "15 slack channels", "20+ skills", "daily briefings"), find a different angle. {{YOUR_NAME}} has more than one story. Draw from product decisions, debugging war stories, team dynamics, competitive observations, tester feedback, {{LAUNCH_EVENT}} prep, privacy community takes, specific tool opinions. If the experience bank (`projects/growsocial/planning/bank.md`) exists, pull from it.
- **No repeat plays within 3 days.** Before drafting, read the last 3 files in `cache/xgrow/` and check their rotation tracking comment. If a play was used in the last 3 days, pick a different one. 33 plays available in `x/plays.md`. Rotating plays keeps the feed unpredictable and avoids pattern detection (both human and algorithmic).

## Trigger

"xgrow" / "x growth" / "draft tweets" / "what should I post" / "find posts to reply to" / "x content"
