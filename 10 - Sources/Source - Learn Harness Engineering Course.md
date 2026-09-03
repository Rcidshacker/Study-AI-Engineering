---
title: Source - Learn Harness Engineering Course
aliases:
  - walkinglabs learn-harness-engineering
  - Learn Harness Engineering
tags:
  - source
  - secondary-source
  - github
  - harness-engineering
  - loop-engineering
  - graph-engineering
source-type: open-source-course
author: walkinglabs
publisher: GitHub
url: https://github.com/walkinglabs/learn-harness-engineering
license: MIT
reliability: medium-high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Learn Harness Engineering Course

> [!info] Verification
> Repository metadata pulled from the GitHub API and the English docs read directly from a
> sparse clone on **2026-09-04**.
>
> | Field | Value (as of 2026-09-04) |
> |---|---|
> | Full name | `walkinglabs/learn-harness-engineering` |
> | Stars | 14,741 |
> | Forks | 1,470 |
> | Language | TypeScript |
> | Licence | MIT |
> | Created | 2026-03-29 |
> | Last push | 2026-08-26 |
> | Open issues | 11 |
> | Topics | agent, agentic, agentic-ai, ai, ai-agent, ai-agents, dsh, dsh-plugin, harness, harness-engineering, harness-framework, llm |
> | English docs | 128 files |
>
> Star counts change. Re-check before quoting.

## Why it is in this vault

It is the only source I found that treats **harness**, **loop**, and **graph** engineering as
one curriculum, with a stated position on how they relate. It is a **secondary** source — it
synthesises the primaries rather than reporting original work — but it is unusually careful
about citing them, and in one case (see the graph section) it does original fact-checking
that corrects the wider discourse.

`[CAUTION]` It is a course with a plugin ecosystem attached (`dsh`, `dsh-plugin` topics) and
a `skills/harness-creator` directory, so it has an interest in the framework being adopted.
It is also the source of several *dates* I could not independently confirm — flagged
individually below and in [[Source - Addy Osmani Loop Engineering]].

## Structure `[FACT]`

- **14 lectures**, each with a `code/` directory of runnable artefacts
- **8 projects**, from "prompt-only vs. minimal harness" to "draw your workflow as a graph"
- **Frontier Harness Design Breakdowns** — reverse-engineering Claude Code, Codex, Pi and
  DeepSeek against the course's own framework
- **Templates** — `AGENTS.md`, `CLAUDE.md`, `claude-progress.md`, `clean-state-checklist.md`,
  `evaluator-rubric.md`, `feature_list.json`, `init.sh`, `session-handoff.md`
- **`tools/audit-harness.sh`** — a harness self-audit script
- 15 language translations

The lecture titles are diagnostic — each names a **failure mode** rather than a feature:

| # | Lecture | Failure it addresses |
|---|---|---|
| 01 | Why capable agents still fail | — |
| 02 | What a harness actually is | — |
| 03 | Why the repository must become the system of record | invisible knowledge |
| 04 | Why one giant instruction file fails | context crowding, rot |
| 05 | Why long-running tasks lose continuity | session amnesia |
| 06 | Why initialization needs its own phase | unprepared environment |
| 07 | Why agents overreach and under-finish | scope drift |
| 08 | Why feature lists are harness primitives | unbounded "done" |
| 09 | Why agents declare victory too early | [[False Completion]] |
| 10 | Why end-to-end testing changes results | [[The Verification Gap]] |
| 11 | Why observability belongs inside the harness | invisible runtime |
| 12 | Why every session must leave a clean state | broken handoff |
| 13 | Loop engineering | human stuck inside the loop |
| 14 | Graph engineering | structural limits of one loop |

`[INFERENCE]` Organising a curriculum by failure mode rather than by feature is itself the
right pedagogy for this field, and mirrors Trivedy's "behaviour we want → harness design"
derivation in [[Source - Anatomy of an Agent Harness]].

## The five-subsystem model `[FACT — the course's own framing]`

> "Harness = Instructions + Tools + Environment + State + Feedback. All five subsystems are
> essential."

With a ranked claim worth testing yourself:

> "Among the five subsystems, the **feedback subsystem** usually has the lowest investment
> and highest return. Get your verification commands right first."

Detail in [[Harness Components]].

## Two methodological contributions

### Controlled-variable ablation `[PRACTICE]`

To find which harness component is actually earning its keep: hold the model fixed, remove
one subsystem at a time, measure the drop. The course is careful about the limit of this:

> "This experiment answers 'which component is most valuable right now' — it cannot, by
> itself, prove 'where the bottleneck is.' To truly locate a bottleneck, you must first
> examine failure records and attributions."

See [[Harness Ablation Testing]].

### The staged case study `[UNVERIFIED — no source given]`

A team building a ~20,000-line TypeScript/React app with GPT-4o, adding harness components
one at a time:

| Stage | Added | Success rate (of 5 runs) |
|---|---|---|
| 1 | README only | 20% |
| 2 | `AGENTS.md` with stack versions, conventions, architecture | 60% |
| 3 | Verification commands listed in `AGENTS.md` | 80% |
| 4 | Progress-file templates | 80–100% |

> "Four iterations, the model did not change at all, and success rate went from 20% to near
> 100%."

**The course names no team, publication, or dataset for this.** n=5 per stage is tiny. Treat
it as an **illustration of the shape of the effect, not as evidence of its size.** The
directionally-similar but *properly sourced* claim is in
[[Source - OpenAI Harness Engineering]]. See [[Harness Beats Model Choice]].

## Lecture 13: loop engineering `[FACT — as reported by the course]`

Its most useful original contribution is a taxonomy of loop *types* by trigger and stop
condition — see [[Loop Types]] — and a clean separation of two commands people conflate:

| | `/goal` | `/loop` |
|---|---|---|
| What it is | one big task, runs until done | one small action, repeats on an interval |
| Stop | goal reached or budget exhausted | you stop it, or the task exits |
| Progress | cumulative — closer each iteration | independent runs, no accumulation |
| Test | *does this thing have an end?* → yes | → no, you just need to keep watching |

And the four-stage history of how autonomous loops emerged, ending at the key insight:
**taking "is it done?" out of the hands of the agent doing the work.** See
[[Generator Evaluator Separation]] and [[Stopping Conditions]].

> [!warning] Verify the product claims yourself
> The course states that Claude Code and Codex "independently shipped `/goal`" in early 2026
> and describes `/loop`, Routines, and thread automation. **Command surfaces change fast.**
> Check the current [[Claude Code]] documentation before building on any specific command.
> What is durable is the *shape*: goal + verification + stopping condition.

## Lecture 14: graph engineering, and the fact-check `[FACT]`

This lecture does something rare and valuable: it debunks its own subject's hype. Full
treatment in [[Graph Engineering Origin and Fact-Check]]. The headline findings:

- The term went viral off a **joke** — Peter Steinberger's "Are we still talking loops or
  have we moved on to graphs yet?" and Hamel Husain's reply article whose entire body was a
  "Stop it" GIF. Both were satirising the industry's six-week term cycle.
- The widely-circulated **"+18% accuracy, −85% cost"** figures are **fake** in this context —
  the numbers exist but come from a paper about **chemical piping diagrams**, against
  different baselines.
- The claim that "Microsoft, Stanford and Anthropic all discovered graph engineering at
  once" is **false**.
- One genuine precursor survives fact-checking: **Josh Simmons**, *We Are Entering the Graph
  Engineering Phase*, dated **July 4** — two weeks before the joke.

> "The joke made the idea trendy. It did not create the idea."

## The four-layer stack `[FACT — course's synthesis of a thread by @rohit4verse]`

| Layer | Shapes | Answers |
|---|---|---|
| Prompt engineering | the instruction | how do we tell the model what to do? |
| Context engineering | the information | what should it know before deciding? |
| Loop engineering | the runtime | how does it iterate until the goal is met? |
| Graph engineering | the system | how do multiple agents, loops, tools and evaluators work together? |

With the crucial reading: **each layer stacks on the one before, it does not replace it.**
And the course's own correction — harness is missing from that list because the thread was
telling a story about buzzwords. The course places **harness as the foundation** that loops
and graphs are built on, and honestly notes the placement is unsettled in the literature.

This directly answers the user's proposed mental model. See [[The Unified Mental Model]].

## How to use this source

- Read **lectures 02, 13, 14** and the **Claude Code harness-design breakdown** first.
- Take the **templates** (`feature_list.json`, `init.sh`, `session-handoff.md`,
  `evaluator-rubric.md`) — they are the fastest on-ramp to a working harness.
- **Chase every date and statistic back to a primary source** before repeating it. This note
  flags the ones I could not confirm.

## Related

- [[Harness Engineering]] · [[Loop Engineering]] · [[Graph Engineering]] · [[The Unified Mental Model]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Anthropic Effective Harnesses for Long-Running Agents]]
- [[Graph Engineering Origin and Fact-Check]] · [[Sources MOC]]
