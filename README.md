# product-os

Running product means synthesizing signal from a dozen directions simultaneously — team, stakeholders, users, market, investors. I built this to do that without losing the thread. Every function runs on a skill: a complete process with required context reads, output structure, and a named failure-mode checklist Claude applies before finalizing. Invoke one, get the output.

Context compounds. OKRs, positioning, team dynamics, writing voice — all in files Claude reads before every session. Each output becomes context for the next. The longer you use it, the sharper it gets.

Output quality is enforced at the skill level. The writing eval covers 25 named patterns across four categories: LLM voice (summary bows, corrective antithesis, parallel triads, hedge words), redundancy (bracket explanations, bold restatements), rhythm (staccato on repeat, cookie-cutter paragraphs), and generic writing. Flagged output gets rewritten before it lands in docs.

The design bets: skills over chat (repeatable beats ad-hoc), files over databases (portable beats locked-in), explicit context over implicit memory (auditable beats opaque). Each bet trades setup cost for compounding return.

## Architecture

The brief skill runs 16+ parallel refresh agents across MCPs, CLIs, and custom browser simulations — one per data source. Each agent writes to a cache file and returns a delta: what changed since the last run. A single synthesis agent then reads all caches plus context files in one pass with a fresh 200K-token context.

Refresh agents are stateless and parallelizable. The synthesis agent gets zero conversation overhead. Clean data, full context.

## What you can do

**Cross-team visibility**
- `/brief` — synthesize signal across teams, sources, and stakeholders into one morning view
- `/status` — track what's moving, stuck, and where you need to intervene across workstreams
- `/meeting-notes` — surface who committed to what, who owes you what, and what the political subtext was
- `/tasks` — surface urgent work across all open threads

**Decision support**
- `/shape` — give form to a fuzzy idea before committing resources to it
- `/feedback` — synthesize signal across users, team, and stakeholders into a prioritized read
- `/users` — track relationships, surface follow-ups, manage the people side of GTM

**Product development**
- `/prd` — PRDs, briefs, checklists, decision logs
- `/roadmap` — roadmap management
- `/ux` — UX audits, funnel analysis, proposals

**Strategic research**
- `/strategy` — executive-grade strategy docs
- `/competitive-analysis` — deep dives, extract learnings
- `/market-research` — landscape scans, sizing, segment research
- `/narrative` — positioning and messaging

**GTM workflow**
Run `/market-research`, `/competitive-analysis`, `/narrative` first. The GTM plan follows from that output. Write it to `docs/gtm/`.

**Stakeholder comms and content**
- `/comms` — leadership updates, investor comms
- `/reply` — voice-matched replies for any channel
- `/xgrow` — daily X/Twitter content engine
- `/lgrow` — weekly LinkedIn content engine
- `/xarticle` — long-form X articles
- `/essay` — structured essays
- `/harvest` — extract experience bank entries from sessions

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
