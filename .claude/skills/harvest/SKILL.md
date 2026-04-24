---

name: harvest
description: Extract experience bank entries and learnings about {{YOUR_NAME}} from any session. Run after any skill — PRD, metrics, meeting notes, strategy, feedback, social — to capture what happened as post-worthy content and to learn how {{YOUR_NAME}} works.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Harvest Skill

## Purpose

Three things in one pass:

1. Turn real work into experience bank entries. The bank grows from lived sessions, not brainstorming. The richest source is the back-and-forth where {{YOUR_NAME}} and Claude design, debug, or improve the 10x/ system — a new skill, a strategy update, an eval overhaul, a tool that didn't work and had to be rethought.

2. Extract durable signals about who {{YOUR_NAME}} is, how he works, what he needs. Write them to memory immediately. Never make him teach the same thing twice.

3. Analyze the session and surface concrete system improvements: output quality, skill design, agent architecture, context layer, workflow. Surfaced in chat only. Not written to any file.

Run explicitly with `/harvest`.

## Reads

- Current conversation context only. No file loading. Observe first, write after.

## Writes

- Full session output: `cache/harvest/[YYYY-MM-DD].md`
- Section A (approved): `projects/growsocial/planning/bank.md` — correct content section
- {{YOUR_NAME}} profile (always, no approval): `projects/about-me.md`
- Standing workflow rules (if applicable): `~/.claude/projects/-Users-{{YOUR_HANDLE}}-Downloads-10x/memory/MEMORY.md`

## Execution

### Step 1: Reflect on the session

Look back at the full back-and-forth. The raw material is in the dialogue, not just the output. Extract observations across ALL of these categories:

**What was built or decided?**
- A PRD was written → what was the hardest call in the spec?
- A strategy was updated → what data forced the update?
- A roadmap was changed → what got killed and why?
- A doc was revised → what assumption turned out to be wrong?

**What was discovered?**
- A metric surprised → in which direction, by how much, why?
- A user said something unexpected → what was it, what did it change?
- A competitor moved → what does it reveal about the space?
- A system failed → what exactly broke, what was the fix?

**What was hard?**
- Something took longer than expected → why?
- Alignment failed → who disagreed, on what, how was it resolved?
- AI got something wrong → what pattern caused it, what changed?
- A decision had to be made without enough data → what was the call?

**What was counterintuitive?**
- Expected X, got Y → specifics matter
- A shortcut that worked → or didn't work
- A constraint that turned out to be useful

**How did the AI collaboration itself go?** (This is often the richest source — including the background noise.)

Separate the conversation into two streams and analyze both:

*Signal* — deliberate decisions, explicit corrections, moments where the direction changed:
- What did {{YOUR_NAME}} catch that the AI missed? The specific failure mode matters.
- What prompt framing changed the output quality and why?
- Where did the AI confidently produce the wrong thing? What was the tell?
- What did {{YOUR_NAME}} have to correct mid-session, and what did the correction reveal about how AI processes instructions?
- Where did parallel agents help — or create new problems?
- What did the AI do that surprised {{YOUR_NAME}} (positively or negatively)?

*Background noise* — the casual, throwaway, quick exchanges that felt routine. These are worth examining too because:
- What felt routine to {{YOUR_NAME}} might be completely foreign to someone not doing this daily
- A one-word correction ("too long," "wrong tone," "redundant") contains a model of quality that most people don't have
- The back-and-forth rhythm itself — how many rounds it took, what kind of feedback unlocked the right output — is a story
- Small throwaway comments often reveal deeply internalized standards that {{YOUR_NAME}} doesn't think of as expertise
- The moment {{YOUR_NAME}} said "we already did that" or "that's redundant" or "wrong account" — those corrections are calibration data about how the system works

**What would teach someone else something?**
- A workflow pattern — obvious or not. The audience bar is a smart person who doesn't do this daily, not {{YOUR_NAME}}. "I run 4 agents every morning" was obvious to {{YOUR_NAME}}. It got 40K views.
- A workflow or system design that solved a real problem
- A heuristic that emerged from trying something and failing
- Something about agent architecture, eval design, prompt structure, tool routing, or memory
- Something about working with humans through AI-generated output (how they react, what they trust, what they reject)

**What changed between the start and end of the session?**
- A belief that was updated
- A tool or system that was rethought
- A rule that was added because something went wrong
- An assumption that was proven wrong by doing it

**What could the system do better next time?**
- What context was missing that {{YOUR_NAME}} had to provide manually?
- What skill or MCP should have been called but wasn't?
- Where did output require rewriting, and what was wrong with the tone, register, or framing?
- What could have been parallelized?
- What file should contain information that was only in {{YOUR_NAME}}'s head?

### Step 2: Filter for post-worthiness

For each raw observation, run two checks:

1. **Specific enough?** Concrete detail: a number, a named tool, a specific failure mode, a before/after, a timing. "We updated the strategy" fails. "Changing the eval prompt from 'does this pass?' to 'find a reason this fails' caught 60% of first-pass failures that confirmatory review missed" passes.

2. **Classify the observation.** Three possible outcomes — nothing gets dropped:

   - **Post candidate (X-worthy / LinkedIn-worthy / both):** A non-practitioner would learn from it. If a smart person outside this daily workflow would find it interesting, it's a post candidate.

   - **Observation:** Obvious to a smart practitioner. Not post-worthy on its own. Worth capturing in the session cache — could be fodder for improving the system or flagging a gap.

   - **Generic:** Generic, abstract, or audience-obvious. No specific number, no named failure, no lived experience behind it. Still captured in cache — don't drop silently.

### Step 3: Output — three sections

**Section A: Post candidates**

Experience bank entries ready to anchor a post. Max 5 per session — better 2 sharp entries than 5 vague ones. Prioritize **Both** (X + LinkedIn) over single-platform.

```
| # | [Next available #] | [Title — what happened in 5-8 words] | [Detail — specific story: what happened, what you tried, what you learned, what rule emerged. Concrete numbers, names, decisions. 2-4 sentences.] | [Post type: original / reply — X-worthy / LinkedIn-worthy / both] |
```

Present each with a one-line reason and seed post:

> **#82 — [Title]**: [Why this is new, what it adds that isn't already in the bank]
> *Seed (X):* "one sentence that could become an X post"
> *Seed (LinkedIn):* "one sentence that could open a LinkedIn post" (omit if X-only)

**Section B: Observations**

Patterns, friction points, or practitioner-obvious things worth capturing. Not post-worthy on their own, but worth seeing — potential fodder for system improvements, skill updates, or future bank entries once they compound.

Format: plain bullet list. One sentence each. Label the type:
- `[system]` — something about how the skill/agent/tool works that could be improved
- `[pattern]` — something that keeps happening across sessions
- `[gap]` — something the current system doesn't handle that it probably should
- `[friction]` — a step that felt slower or harder than it should

**Section C: Generic**

Observations that are real but too abstract or audience-obvious to be post-worthy. Kept so no analysis is wasted.

Format: plain bullet list. No labels. One sentence each.

**Section D: System improvements (chat only, not written to any file)**

Analyze the session and suggest concrete improvements to how AI was used. The goal: higher quality output, more parallelization, more human-sounding results, fewer round-trips. Assess across 5 dimensions:

- **Output quality** — Did the output sound human or AI? What triggered {{YOUR_NAME}} to rewrite? What was wrong with the tone, register, or framing? Determine this from what happened in the session (corrections, rewrites, rejections), not by cross-referencing rule files.
- **Skill design** — Could the skill that ran have been structured differently? Missing steps, wrong ordering, missing reads/writes, prompts that didn't produce the right output on first pass?
- **Agent architecture** — Could work have been parallelized? Were agents used when direct tool calls would've been faster (or vice versa)? Did agent context get wasted? Were too many/few agents launched?
- **Context layer** — Were the right MCPs called? Was stale cache used when fresh data was available? Were docs/context files read that should have been? Were any missing?
- **Workflow** — Could steps have been reordered? Did {{YOUR_NAME}} have to provide context the system should have known? Were there unnecessary round-trips? Could the session have been shorter?

Each improvement must be:
- **Specific**: names the file, skill, MCP, or pattern
- **Actionable**: says exactly what to change
- **Scoped**: targets one thing, not "make everything better"

Format: plain bullet list. Tag each with `[output]`, `[skill]`, `[agent]`, `[context]`, or `[workflow]`. Bold the title. One sentence explanation. If a dimension has no improvement, say so in one line ("No parallelization opportunity — session was sequential by nature").

Surface Section D in the chat message to {{YOUR_NAME}}. Do not write it to the cache file or any other file. {{YOUR_NAME}} decides what to act on.

### Step 4: Write

**4a. Write full session → cache (always, immediately):**
- Write the complete session output (all three sections) to `cache/harvest/[YYYY-MM-DD].md`
- If a file for that date already exists, append a new `## Session [HH:MM]` block
- Cache file format:

```
# Harvest — [YYYY-MM-DD]

## Session [HH:MM] — [1-line description of what session was about]

### A: Post candidates
[full Section A output]

### B: Observations
[full Section B output]

### C: Generic
[full Section C output]
```

**4b. Write Section A → experience bank (automatic, no approval gate):**
- Write all Section A entries immediately after generating them. Do not ask for confirmation.
- Add to the correct section in `projects/growsocial/planning/bank.md`
- Match the section to the content type (Product Decisions, AI Leadership, AI Content System, etc.)
- If no existing section fits, create one
- Update the entry number to be sequential from the last entry

**4c. Extract learnings about {{YOUR_NAME}} → memory (no approval needed):**

Every harvest session is an input about who {{YOUR_NAME}} is, how he works, what he needs. Before closing:

- Scan the full conversation — corrections, decisions, friction, what he pushed back on, what he accepted without question, throwaway comments that reveal standards.
- Write immediately. No confirmation. No surfacing for approval.
- Route to `projects/about-me.md` — persistent profile: who he is, how he operates, what he values, what he needs from AI. Update the relevant section; don't append blindly.
- If it's a standing workflow rule worth loading every session, also write to `~/.claude/projects/-Users-{{YOUR_HANDLE}}-Downloads-10x/memory/MEMORY.md`.
- **Never make {{YOUR_NAME}} teach the same thing twice.**

## What this is NOT

- Don't harvest generalizations or observations you could have made without doing the session
- Don't harvest things {{YOUR_NAME}} already knows and wouldn't find surprising
- Don't harvest things that are confidential or that {{YOUR_NAME}} explicitly said not to post (Sonium, internal org dynamics, names of specific investors/partners without context)

## Trigger examples

Run `/harvest` after any of these:

**Product/strategy work:**
- Finished a PRD → what was the tradeoff that wasn't obvious at the start?
- Reviewed Mixpanel/Amplitude → what metric moved unexpectedly?
- Processed meeting notes → what decision got made in a room that changed the roadmap?
- Ran a user interview synthesis → what did users say that contradicted the assumption?
- Updated a strategy doc → what forced the update?

**AI/agent system work:**
- Ran lgrow or xgrow → what eval failure was new? what angle worked?
- Built or updated a skill → what constraint forced a different architecture?
- Debugged an agent → what was the agent confidently wrong about and why?
- Redesigned an eval → what was the old eval missing?
- Added a new tool or MCP → what broke before it worked?

**Learning/insight moments:**
- Researched a creator or competitor → what technique was non-obvious?
- Found something that contradicted a prior assumption about how AI works
- Had to correct AI multiple times on the same thing → what does the pattern reveal?
- Discovered a system you built has a structural flaw → what's the flaw and what's the fix?
- Built something in 10 minutes that used to take 3 days → be specific about both

**Human-AI collaboration:**
- Had a conversation with a human (Slack, Signal, email) that then informed an AI session → what was the translation?
- Shipped an AI-generated output to a human → how did they react to it? what did they flag?

---

*Created: 2026-03-07. Updated: 2026-03-24*
