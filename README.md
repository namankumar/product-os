# product-os

An AI harness for product work. 21 skills across the full product lifecycle. Fork it, fill in your context, and a small team runs like a much larger one.

Skills capture how great product work gets done: research before a decision, narrative before a pitch, user synthesis. Each skill is a complete process: what to read, how to think, what to produce. Invoke one, get the output.

Context compounds. OKRs, positioning, team dynamics, writing voice. All in files Claude reads before every session. Each output becomes context for the next. The longer you use it, the more capable it gets.

Output quality is enforced at the skill level. Each skill defines output structure, required context reads, and a failure-mode checklist Claude applies before producing final output. The writing eval covers 25 named patterns across four categories: LLM voice (summary bows, corrective antithesis, parallel triads, hedge words), redundancy (bracket explanations, bold restatements), rhythm (staccato on repeat, cookie-cutter paragraphs), and generic writing. Flagged output gets rewritten before it lands in docs.

The design bets: skills over chat (repeatable beats ad-hoc), files over databases (portable beats locked-in), explicit context over implicit memory (auditable beats opaque). Each bet trades setup cost for compounding return.

## What you can do

**Daily operations**
- `/brief` — morning brief across all data sources, or weekly review
- `/status` — workstream tracking, blockers, dependencies
- `/meeting-notes` — extract decisions, commitments, people signals from transcripts
- `/tasks` — organize todos, surface urgent work

**Product**
- `/prd` — PRDs, briefs, checklists, decision logs
- `/roadmap` — roadmap management
- `/feedback` — collect, triage, prioritize
- `/users` — user relationship tracking
- `/ux` — UX audits, funnel analysis, proposals
- `/shape` — give form to a fuzzy idea before committing to a PRD

**Strategy and research**
- `/strategy` — executive-grade strategy docs
- `/competitive-analysis` — deep dives, extract learnings
- `/market-research` — landscape scans, sizing, segment research
- `/narrative` — positioning and messaging

**Comms and content**
- `/comms` — leadership updates, investor comms
- `/reply` — voice-matched replies for any channel
- `/xgrow` — daily X/Twitter content engine
- `/lgrow` — weekly LinkedIn content engine
- `/xarticle` — long-form X articles
- `/essay` — structured essays
- `/harvest` — extract experience bank entries from sessions

## How it works

```
You                Claude Code              Workspace
───                ───────────              ─────────
/brief          →  reads brief skill    →   context/ (OKRs, narratives, cadences)
                   runs data sources    →   cache/ (scan outputs)
                   writes output        →   docs/daily/YYYY-MM-DD.md

/prd            →  reads prd skill      →   context/ (product context)
                   reads existing PRDs  →   docs/product/
                   writes new PRD       →   docs/product/prd-feature.md

/strategy       →  reads strategy skill →   context/ + docs/market-research/
                   synthesizes          →   docs/strategy/strategy-topic.md
```

Claude Code runs the skill. The workspace is the memory.

## Brief architecture

The brief skill runs 16 parallel refresh agents, one per data source, then a single synthesis agent with a fresh 200K-token context. The refresh agents write to cache files and return a delta: what changed since the last run. The synthesis agent reads all 16 caches plus all context files in one pass and cross-references across them.

Refresh agents are stateless and parallelizable. The synthesis agent gets zero conversation overhead. Just clean data and context.

## How to fork

1. Fork this repo
2. Open `CLAUDE.md` — replace all `{{PLACEHOLDERS}}` with your context
3. Fill in `context/current-context.md` with what's true right now
4. Add your OKRs to `context/okr-status.md`
5. Install [Claude Code](https://claude.ai/code)
6. Run `/brief`

## Structure

```
product-os/
├── CLAUDE.md              # Who you are, your team, your product — Claude reads this first
├── tasks-db.md            # Open todos
├── .claude/
│   └── skills/            # Skill definitions
├── context/               # Ground truth — Claude reads before every session
│   ├── current-context.md
│   ├── okr-status.md
│   ├── narrative-map.md
│   ├── cadences.md
│   ├── artifacts.md
│   └── room/
├── docs/                  # Produced output
│   ├── strategy/
│   ├── narrative/
│   ├── gtm/
│   ├── market-research/
│   ├── product/
│   └── users/
├── cache/                 # Machine-generated scan data
└── projects/
```

