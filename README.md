# product-os

An AI harness for product work. Skills, context, and output — structured so Claude Code can operate as a capable thought partner, not just a code assistant.

Fork it. Replace the placeholders. Start working with an AI that knows your product, your team, and how you think.

---

## What this is

Most people use Claude as a chat window. Ask a question, get an answer, move on. The context resets every session. The AI never accumulates knowledge of your product, your org, or your voice.

This system is different. It's a workspace — a structured set of files that Claude reads before every session. Skills tell Claude how to do specific types of work. Context files tell it what's true right now. Output lands in organized docs that compound over time.

The result: an AI that operates like a senior colleague, not a search engine. It knows your OKRs, your positioning, your team dynamics, your writing style. It can draft a strategy doc, run competitive research, prep for a board meeting, or track user relationships — and do each one consistently, the way you'd want it done.

We chose markdown files over a database or GUI because the output needs to be portable and version-controlled. The day you switch tools or fork the repo, your entire institutional memory is plain text. A proprietary workflow tool locks you in. This doesn't.

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

Claude Code is the execution layer. The workspace is the memory. Skills are the interface.

## Structure

```
product-os/
├── CLAUDE.md              # Who you are, your team, your product — Claude reads this first
├── tasks-db.md            # Open todos
├── .claude/
│   └── skills/            # Skill definitions — the core of the system
│       ├── brief          # Daily/weekly briefing
│       ├── strategy       # Strategy docs
│       ├── prd            # PRDs and specs
│       ├── competitive-analysis
│       ├── narrative      # Positioning and messaging
│       ├── market-research
│       ├── status         # Workstream tracking
│       ├── meeting-notes  # Transcript processing
│       ├── feedback       # Feedback collection and triage
│       ├── users          # User relationship tracking
│       ├── comms          # Leadership and investor updates
│       ├── tasks          # Todo management
│       ├── roadmap        # Product roadmap
│       ├── shape          # Idea shaping
│       ├── ux             # UX audits and proposals
│       ├── reply          # Voice-matched replies
│       ├── xgrow          # X/Twitter content engine
│       ├── lgrow          # LinkedIn content engine
│       ├── xarticle       # Long-form X articles
│       ├── essay          # Essay writing
│       ├── harvest        # Experience bank extraction
│       └── utils/
│           ├── voice.md            # Writing voice and anti-LLM rules
│           ├── writeDocs.md        # Doc editing and narrative rules
│           └── google-docs-formatting.md
├── context/               # Ground truth — Claude reads before every session
│   ├── current-context.md # What's happening now, what to prioritize
│   ├── okr-status.md      # OKR targets vs actuals
│   ├── narrative-map.md   # Active narratives, gaps, what to push
│   ├── cadences.md        # Recurring events and rhythms
│   ├── artifacts.md       # Key external doc registry (Google Docs, Sheets)
│   └── room/              # Group dynamics analysis
├── docs/                  # Produced output (strategy, PRDs, research)
│   ├── strategy/
│   ├── narrative/
│   ├── gtm/
│   ├── market-research/
│   ├── product/
│   └── users/
├── cache/                 # Machine-generated scan data (ephemeral)
└── projects/              # Personal projects, separate from work
```

## Skills

Skills are markdown files. Each one defines a complete process — what to read, how to think, what to produce. Invoke them with `/skill-name` in Claude Code, or reference them in conversation.

**Writing & strategy**
- `/strategy` — executive-grade strategy docs with structured frameworks
- `/prd` — PRDs, briefs, checklists, decision logs
- `/narrative` — positioning and messaging docs
- `/essay` — expressive, well-reasoned essays with a six-doctrine arc system
- `/shape` — take a fuzzy idea and give it form before committing to a PRD

**Research & intelligence**
- `/competitive-analysis` — deep dives on competitors, extract learnings
- `/market-research` — landscape scans, market sizing, segment research
- `/ux` — UX audits, competitive benchmarks, funnel analysis

**Operations**
- `/brief` — daily morning brief or weekly review (cross-references all data sources)
- `/status` — workstream tracking across projects, dependencies, blockers
- `/meeting-notes` — scan email, fetch transcripts, extract decisions and commitments
- `/tasks` — organize todos, surface urgent work
- `/roadmap` — product roadmap management
- `/feedback` — collect, triage, and prioritize feedback
- `/users` — user relationship tracking and follow-ups
- `/comms` — leadership updates, investor comms

**Social & content**
- `/xgrow` — daily X/Twitter content engine
- `/lgrow` — weekly LinkedIn content engine
- `/xarticle` — long-form X articles
- `/reply` — voice-matched replies for any channel
- `/harvest` — extract experience bank entries from sessions

## How to fork

1. Fork this repo
2. Open `CLAUDE.md` and replace all `{{PLACEHOLDERS}}` with your context
3. Fill in `context/current-context.md` with what's true right now
4. Add your OKRs to `context/okr-status.md`
5. Install [Claude Code](https://claude.ai/code)
6. Run `/brief` to see it work

That's it. The system starts working immediately. It compounds as you add more context and produce more output.

## What it's not

This isn't an automation or an agent that runs on a schedule. It's a harness — Claude does the work when you ask, in the way you've defined, with the context you've provided. You stay in control of every decision. The AI does the research, drafting, synthesis, and tracking.

## The future

[Alloy](https://github.com/namankumar/alloy) is the GUI and web layer for this system — a collaborative workspace where skills, context, and output are accessible from a browser, shareable with teammates, and editable without a terminal. product-os is the foundation. Alloy is where it goes next.

---

*Built with [Claude Code](https://claude.ai/code). Powered by your own API keys.*
