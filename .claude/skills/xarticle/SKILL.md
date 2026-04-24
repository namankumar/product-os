---
name: xarticle
description: Write long-form X articles. Research-backed structure, evidence-dense, designed for dwell time and bookmarks.
---

# X Article Skill

## Purpose

Write long-form articles for X that build authority, get bookmarked, and generate follows. Articles are a different format from tweets, threads, and essays. They're reference material people return to and cite. X's algorithm heavily rewards articles through dwell time (+10x for 2+ min reading), bookmarks (2-3x weight), and profile clicks (12x weight).

This is not xgrow. xgrow is daily (replies + 1 original). This runs twice per week, one article per session. xgrow invokes this skill automatically when the last article is 3+ days old.

## Reads

- `.claude/skills/utils/voice.md` — mandatory before every draft
- `projects/growsocial/x/voice.md` — X-specific voice overrides
- `projects/growsocial/planning/quality-gates.md` — shared pre-generation gates
- `projects/growsocial/planning/bank.md` — experience bank (article must be sourced from here)
- `projects/growsocial/planning/weekly-theses.md` — thesis bank
- `projects/growsocial/x/strategy.md` — content pillars, positioning
- `projects/growsocial/x/algorithm.md` — algorithm signals
- `projects/growsocial/x/inspiration-bank.md` — viral tweet examples by play. Reference article-format examples before drafting.
- `projects/growsocial/x/article-topics.md` — persistent topic pipeline (check first before generating candidates)
- `cache/xarticle/` — prior articles (avoid repeating topics)
- `cache/xgrow/` — last 5 daily files (know what's been said as short-form, don't repeat)
- `context/current-context.md` — if topic touches active work
- `docs/` — institutional knowledge for evidence sourcing

## Writes

- `cache/xarticle/[YYYY-MM-DD]-[topic-slug].md` — article draft + metadata

## Trigger

`/xarticle` or "write an x article" or "draft an article for x"

---

## What Wins on X Articles (extracted from $2.15M contest winners)

These are patterns from the actual winning articles, not theory.

### The one rule

**The article IS an asset.** If someone else could write it with a Google search, it won't perform. Every winning article had something irreplicable: a self-built database (beaverd, $1M), 12 months of pattern tracking (Kobeissi, $500K), personal identity + historical scholarship (Asiedu, $100K), VC-grade cross-domain analysis (Wolfe, $100K), physical investigation footage (Shirley, $100K).

For {{YOUR_NAME}}: the irreplicable asset is the intersection of AI-native product workflow + crypto/privacy domain + specific numbers from real shipping. Nobody else has run 4 parallel Claude agents every morning for months while shipping a privacy wallet.

### Algorithm advantage

| Signal | Weight | Why articles win |
|---|---|---|
| Dwell time (2+ min) | +10x | Articles generate minutes, not seconds |
| Bookmarks | 2-3x | Reference material gets saved |
| Profile clicks | 12x | Authority content drives "who is this?" |
| Follows after read | 25x | Strongest signal, articles convert |
| Replies | 27x | Companion thread generates discussion |

### What doesn't matter

- Follower count (beaverd won $1M with 90K followers)
- Beautiful writing (conversational tone won, not formal)
- Length alone (density matters, not word count)
- Being contrarian (only 1 of 6 winners was truly contrarian)

---

## Two Winning Structures

Not one template. Two distinct structures. Pick the one that fits the topic.

### Structure A: The Investigation

Used by beaverd ($1M), Shirley ($100K). Best when you have original data or research.

```
HIDDEN TRUTH HOOK
  "You were never meant to know [specific thing]"
  State what the article proves. Don't tease.

SCALE
  How big the problem/pattern is. Numbers.

REPEATED CASE STUDIES
  3-5 specific examples with a rhetorical refrain.
  beaverd: "Built by Deloitte" repeated across state failures.
  Each case: specific dollar amount + specific human cost.

THE MECHANISM
  Why this keeps happening. Systemic explanation.
  Not "it's broken." HOW it stays broken.

VERIFIABLE PROOF
  Link to data, downloadable resource, or source material.
  beaverd: somaliscan.com with CSVs.
  The article points to something readers can verify.
```

**When to use:** You audited something (14 onboarding flows), built a dataset, investigated a pattern across multiple examples. The evidence is the article.

### Structure B: The Framework

Used by Kobeissi ($500K), Wolfe ($100K). Best when you have a predictive model or unifying thesis.

```
REFRAME HOOK
  Bold reframe of something everyone's watching.
  State the claim upfront. The surprise is in the claim.
  Kobeissi: "We spent 12 months analyzing EVERY tariff. Here's the exact playbook."

CREDENTIALS
  1 paragraph. Why you specifically see this pattern.
  Not resume. The specific experience that gave you the lens.

NUMBERED FRAMEWORK
  3-10 steps/sections, each self-contained.
  Each section: the step + evidence for that step.
  Specific dates, specific numbers, specific names at every point.
  No filler sections. Every section adds new evidence.

THE TURN
  What most people miss. The non-obvious implication.
  Or: where the framework breaks down / its limitation.

FORWARD APPLICATION
  What happens next. Scenarios with probability weights.
  Kobeissi: 3 scenarios for next 2-4 weeks.
  Or: what the reader should do with this framework.
```

**When to use:** You've identified a pattern, built a mental model, have a framework others can apply. The insight is the article.

### What both structures share

- **Hook states the claim, doesn't tease.** The reader clicks because the claim is surprising, not because the answer is hidden.
- **Rhetorical repetition as structural device.** A phrase or pattern that repeats across sections. Makes the article skimmable and gives readers language to cite when sharing.
- **Designed to be referenced after publication.** Kobeissi tweeted "we are now on step #5" for weeks after. The article generates ongoing engagement. Before drafting, answer: "How will this article be cited after publication?"
- **Evidence density, not word count.** Named people, specific dollar amounts, specific dates, prediction market spreads. No filler paragraphs.
- **Conversational tone.** "Ding ding ding" (beaverd). "Here's the exact playbook you need" (Kobeissi). First person. Not formal, not blog-style.

---

## Process

### Step 0: Topic selection (or confirm user's topic)

**If no topic given**, check `projects/growsocial/x/article-topics.md` first. Queued topics there are pre-approved candidates. Pick from the queue before generating new candidates.

If the queue is empty or nothing fits, surface 3-5 candidates. Each gets:

1. Topic (one sentence)
2. The irreplicable asset — what do you have that nobody else can write?
3. **Broadness check** — would a stranger outside crypto/AI/product management find this useful? Articles force a 5-10 minute commitment from the reader. The topic must justify that commitment to a wide audience. "How delegated proving leaks privacy" fails (audience is ~200 people). "What happens when you replace your entire workflow with AI" passes (any builder relates). If the topic is deep but narrow, use a thread instead.
4. Structure fit — Investigation (A) or Framework (B)?
5. Bank entries that feed it (by number)
6. Reference hook — how will this be cited after publication?

Sources for candidates:
- `projects/growsocial/x/article-topics.md` — queued topics (check first)
- Experience bank themes with 3+ entries on the same topic
- Weekly theses that have been explored in short-form and are ready for depth
- Gaps in `cache/xarticle/` (topics not yet covered)
- Miner output from recent xgrow runs (sharp lines that deserve expansion)

**{{YOUR_NAME}} picks one.** Don't draft until confirmed.

**If topic is given**, confirm the irreplicable asset, broadness, and structure fit before proceeding.

### Step 1: Asset audit

Before any writing, answer these three questions. Present to {{YOUR_NAME}}:

1. **What is the irreplicable asset?** (dataset, framework, insider access, personal experience with specific numbers)
2. **What's the reference hook?** (How will {{YOUR_NAME}} cite this article in future tweets? "As I wrote in my article on X..." or sharing a specific section as a screenshot)
3. **What bank entries feed this?** (List specific entry numbers from bank.md)

If the asset is weak ("I think X is true"), the article isn't ready. Surface what research or data gathering would strengthen it. Don't force articles from pure opinion.

### Step 2: Hook generation

Articles have **two hooks**, not one. They serve different functions:

1. **Title + cover image (feed-level hook).** This is what appears in the X feed as a card. The reader sees the TITLE and the IMAGE, not the body text. The title must make a 5-10 minute read feel worth it to a stranger scrolling past. This is the entire distribution strategy. Score it with H1-H4 below.
2. **First 2-3 lines (body hook).** This is what the reader sees AFTER clicking. It keeps them reading. It states the claim and establishes credentials. If the title gets the click but the first paragraph doesn't hold, the reader bounces and dwell time tanks.

**The core mechanic for both:** The surprise is in the claim, not in hiding the answer. The reader clicks because what you're claiming sounds surprising AND evidence-backed, not because you withheld something.

Article hooks are different from tweet hooks and LinkedIn hooks. They don't create an unresolved gap (tweet mechanic) or force a binary reaction (LinkedIn mechanic).

**Title rules:**
- Title IS the hook. Treat it as the most important line in the article.
- Must pass H1-H4 scoring on its own (not just the body text).
- Conversational tone, not formal. "the science behind going viral on X [a practical guide]" outperforms "A Comprehensive Guide to X Algorithm Optimization."
- Brackets for subtitle/qualifier work well: "[a practical guide]", "[with data]", "[what nobody tells you]"

#### Article Hook Types

**Type 1: Hidden Truth**
You found something most people don't know exists. The hook reveals that it exists and states what you found.

Pattern: `"You were never meant to know [specific thing]. [Scale of what you found]."`

- beaverd: "You were never meant to hear the name 'Deloitte' and you were never meant to know that the government has wasted $74 billion by working with them."
- {{YOUR_NAME}} example: "I audited 14 stablecoin wallet onboarding flows. Not one designs for someone with $200."

Best for: Structure A (Investigation). Requires original data or research.

**Type 2: Exhaustive Research Claim**
You did more work than anyone else on this topic. The hook signals the depth of evidence behind the article.

Pattern: `"I/We spent [time/effort] analyzing [scope]. Here's [what you found / the framework]."`

- Kobeissi: "We spent 12 months analyzing EVERY tariff development, here's the exact tariff playbook you need."
- {{YOUR_NAME}} example: "I've run 4 parallel AI agents every morning for 6 months. Here's what actually disappeared from my workflow."

Best for: Structure B (Framework). The time investment IS the credential.

**Type 3: Unification Reframe**
Multiple things everyone sees as separate are actually one thing. The hook collapses complexity into a single thesis.

Pattern: `"What looks like [N separate things] is actually [one thing]."`

- Wolfe: What the average observer sees as three separate stories are actually "three fronts of a single conflict."
- {{YOUR_NAME}} example: "Everyone's debating AI speed vs AI accuracy vs AI cost. It's one variable: how many times you've already run the loop."

Best for: Structure B (Framework). Requires cross-domain pattern recognition.

**Type 4: Contradiction Hook**
Your personal experience contradicts what everyone assumes. The hook states the contradiction.

Pattern: `"I [did the thing everyone recommends]. [Unexpected result]."`

- Asiedu: America is "the best place to be a black person on earth" yet the narrative says otherwise.
- {{YOUR_NAME}} example: "I replaced my entire product workflow with AI. The job got harder, not easier."

Best for: Either structure. The contradiction is the article's spine.

**Type 5: Predictive Framework**
You've identified a pattern that predicts what happens next. The hook offers the pattern as a tool.

Pattern: `"Here's the exact [playbook/pattern/framework] for [thing everyone's watching]."`

- Kobeissi: "President Trump's EXACT Tariff Playbook, A Step-by-Step Guide"
- {{YOUR_NAME}} example: "The exact sequence every privacy product follows before it either works or dies. We're on step 4."

Best for: Structure B (Framework). The article is a prediction engine readers can cite as events unfold.

#### Hook Scoring (article-specific)

Score every hook before drafting. Gate: 7/10 required. Max 3 iterations before reconsidering the topic.

| Dimension | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| **H1: Claim strength** | No claim. Vague topic announcement. | Mild claim. Reader thinks "ok, maybe." | Strong claim. Reader thinks "really?" | Extraordinary claim. Reader thinks "I need to see the evidence." |
| **H2: Evidence signal** | No hint of evidence behind the claim. | Implies some experience. | Names the specific evidence (number, timeframe, scope). | Evidence scope is itself surprising ("600M rows", "12 months", "14 flows"). |
| **H3: Time commitment justified** | Reader wouldn't spend 5 min on this. | Reader might skim. | Reader expects the article to be worth the full read. | Reader would bookmark before reading. |
| **H4: Specificity** | Abstract. Could be about anything. | Named topic but no concrete detail. | Specific number or named thing in the hook. | Multiple specifics that compound (number + named thing + verdict). |

**Total: 0-11. Gate: ≥7.**

**Hard fails (auto-reject regardless of score):**
- Hook teases without stating the claim ("You won't believe what I found...")
- Hook is a question without a position ("Is AI changing product work?")
- Hook could be written by anyone without the experience ("AI is transforming workflows")
- Hook previews the article structure ("In this article, I'll cover 5 things...")
- Hook uses LLM tells from voice.md

#### Hook Generation Process

1. Write 3-5 candidate hooks, each a different type from above
2. Score each against H1-H4
3. Pick the highest-scoring hook that passes the gate
4. If none pass, the topic may not be article-ready. Reconsider the asset.

Present the top 2 hooks to {{YOUR_NAME}} with scores. He picks.

### Step 3: Structure selection

Pick Structure A (Investigation) or Structure B (Framework) based on the asset:

- Original data / audited multiple things / built a dataset → **Structure A**
- Pattern recognition / predictive model / mental model → **Structure B**
- Both elements present → use the one where the evidence is stronger

State the structure choice and map the sections before drafting:

**For Structure A:**
- Hook claim (one sentence)
- Scale evidence (what numbers establish scope)
- Case studies (list 3-5 with the refrain phrase)
- Mechanism (the systemic "why")
- Proof resource (what readers can verify)

**For Structure B:**
- Reframe claim (one sentence)
- Credential (the specific experience)
- Framework steps (list each with its evidence)
- Turn (the non-obvious part)
- Forward application (scenarios or actions)

### Step 4: Draft

Load voice.md + x/voice.md + quality-gates.md before writing.

**Drafting rules:**

- **2,000-3,000 words.** Every winner was in this range. Under 1,500 = use a thread instead. Over 4,000 = you're padding.
- **Short paragraphs.** 1-3 sentences. No walls of text.
- **Bold key phrases** for skimmers. Someone should get the argument from bolded text alone.
- **Headers create the skeleton.** Reader should get the thesis from headers alone.
- **Embedded evidence at every claim.** Screenshots, specific numbers, named people, dates. No unsupported assertions.
- **First person singular.** "I found" not "we analyzed."
- **Conversational tone.** Write like explaining to a sharp friend, not lecturing. voice.md applies fully.
- **One linguistic device** from voice.md, selected before drafting. Device shapes the article's rhythm.
- **No product names.** Same rule as xgrow. "Privacy wallet" not "{{PRODUCT}}." Exception: if the article is explicitly about {{PRODUCT}} and {{YOUR_NAME}} approves.

**Anti-slop gate (mandatory):**
All voice.md kill rules apply. Additionally:
- No summary paragraphs. Each section adds new evidence, never recaps.
- No "in this article, we'll explore..." or any preview of structure.
- No gift-wrapped ending. Close with forward application or verifiable proof, not a quotable summary line.
- Vary paragraph length. Mix 1-sentence paragraphs with 3-sentence ones.

### Step 5: Companion thread draft

Every article needs a companion thread for distribution. Draft it alongside the article.

**Thread structure (3-5 tweets):**
1. Hook tweet: the single most surprising finding from the article. Not "I wrote an article about X." The finding itself, standalone.
2. 2-3 middle tweets: one key insight per tweet, each screenshottable alone.
3. Final tweet: link to the article. "Full breakdown:" + link.

The thread is not a summary. It's the highlight reel that makes people want the full version.

### Step 6: Self-review

Run against these evals before presenting. Fix internally. Don't surface failures unless structural.

**Article-specific evals:**

| # | Eval | Pass criteria |
|---|---|---|
| A1 | Irreplicable asset present | Could a stranger write this from Google? If yes, fail. |
| A2 | Hook states claim | First 2-3 lines state what the article proves. No teasing. |
| A3 | Evidence density | Every section has at least one: specific number, named person, specific date, or verifiable fact. |
| A4 | Reference reusability | Can {{YOUR_NAME}} cite a specific section in a future tweet? Is there a framework/finding worth screenshotting? |
| A5 | Completion incentive | Would a reader who starts keep reading? Each section must create a reason to read the next. (D2 from essay skill) |
| A6 | Forward application | Article closes with what happens next, what the reader should do, or where to verify. Not a summary. |
| A7 | Voice match | Would {{YOUR_NAME}} send this to a friend? Read sections aloud. If any sound "written," rewrite. |
| A8 | Dwell time optimization | Is the article dense enough to hold attention for 3+ minutes but not so long it loses readers? |
| A9 | Bank sourced | Every personal claim traces to a specific bank entry number. |

**Also run:** All voice.md kill rules, anti-slop gate from xgrow Step 7g, quality-gates.md shared checks.

### Step 7: Present

Output the article draft + companion thread + metadata.

---

## Output Format

```markdown
# X Article — [Topic Slug]

**Date:** [YYYY-MM-DD]
**Structure:** [A: Investigation / B: Framework]
**Asset:** [one sentence — what makes this irreplicable]
**Bank entries:** [#numbers used]
**Device:** [linguistic device chosen]
**Reference hook:** [how this gets cited in future tweets]
**Word count:** [number]

---

## Article

[Full article text with headers]

---

## Companion Thread

**Tweet 1 (hook):**
> [text]

**Tweet 2:**
> [text]

**Tweet 3:**
> [text]

**Tweet 4 (link):**
> [text + article link placeholder]

---

## Distribution Checklist

- [ ] Publish article on X (desktop only)
- [ ] Post companion thread immediately after
- [ ] Pin article for 48-72 hours
- [ ] Engage every reply — not just the first hour. Keep replying for as long as comments come in. Each reply is the highest-weight algorithmic signal. Even an emoji counts. Data point (Chua): 10+ hours replying to a single viral post.
- [ ] **Check views at hour 4-6.** If below expected pace, self-QT with a different angle (not a summary, not "icymi" — a standalone one-liner that reframes the article from a new entry point). Quote views count toward the original's total impressions. Data point (Chua): article at 5K views after 6 hours → self-QT → 400K impressions on the quote. This is the highest-leverage rescue move for slow-starting articles.
- [ ] Reshare at 1 week
- [ ] Reshare at 1 month
- [ ] Cross-post key insight to LinkedIn (lgrow)

---

## Evals

| Eval | Result |
|---|---|
| A1: Irreplicable asset | [Pass/Fail + note] |
| A2: Hook states claim | [Pass/Fail] |
| A3: Evidence density | [Pass/Fail] |
| A4: Reference reusability | [Pass/Fail + the citable section] |
| A5: Completion incentive | [Pass/Fail] |
| A6: Forward application | [Pass/Fail] |
| A7: Voice match | [Pass/Fail] |
| A8: Dwell time | [Pass/Fail] |
| A9: Bank sourced | [Pass/Fail + entry numbers] |
```

---

## Rules

- **One article per session.** Don't batch.
- **Never post on {{YOUR_NAME}}'s behalf.** Output is a draft. Always.
- **No product or chain names** unless {{YOUR_NAME}} explicitly approves for that article.
- **Technical accuracy is mandatory.** Cross-check against CLAUDE.md glossary. Don't invent numbers.
- **Don't force it.** If the asset isn't strong enough, say so. Recommend what research or data gathering would make it ready. A weak article hurts more than no article.
- **Articles are not expanded threads.** Different structure, different density, different purpose. If the topic works as a thread, write a thread.
- **Articles are not essays.** Essays argue a thesis through narrative arc. Articles present evidence and frameworks through structure. Use `/essay` for thesis-driven persuasion. Use `/xarticle` for evidence-driven reference material.
- **X-specific formatting:** Use headers (H2, H3), bold, bullet lists, embedded images where possible. The article must be scannable. No walls of unbroken prose.
- **Desktop only.** Articles can only be created on x.com desktop. Note this in the output.
- **Editing limitation.** Must unpublish to edit, then republish. Get it right before publishing. Note this in the output.

## Cadence

- Twice per week. xgrow invokes automatically when last article is 3+ days old.
- Monday and Thursday publishing preferred.
- Can also be triggered standalone via `/xarticle`.

## Relationship to Other Skills

- **xgrow** → surfaces article-ready topics, generates short-form content that tests angles before committing to long-form. xgrow's `## Article Draft` section in daily files is replaced by this skill.
- **essay** → different format. Essays are thesis-driven persuasion with narrative arc (D1-D6). Articles are evidence-driven reference material with structural clarity. Different evals, different structure, different purpose.
- **lgrow** → cross-post key insights from articles to LinkedIn. The article is the source, LinkedIn post is derivative.
- **harvest** → after publishing, harvest bank entries from the article's performance and reception.
