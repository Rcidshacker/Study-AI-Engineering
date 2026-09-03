---
title: Generator Evaluator Separation
aliases:
  - Maker checker
  - Maker-checker split
  - Separate the judge from the judged
tags:
  - loop-engineering
  - verification
  - graph-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Generator Evaluator Separation

> [!abstract] One line
> The thing that did the work must not be the thing that decides the work is done.

This is the highest-leverage single idea in loop engineering, and the one most often skipped
because it costs an extra model call.

---

## Why it is necessary, not merely nice

`[FACT]` Agents "confidently praise their own work" — reported by Anthropic and summarised in
[[Source - Learn Harness Engineering Course]].

`[FACT]` Anthropic's own documented failure mode: a later agent session "would look around,
see that progress had been made, and declare the job done"
([[Source - Anthropic Effective Harnesses for Long-Running Agents]]).

`[FACT]` And the structural explanation, from Lecture 14 of the course: a loop's internal
checkpoints cannot fix this because **"the judge and the judged share one brain and one
context."** A self-check will stop the agent shipping without *running* a test. It will not
ask *"is this the right test?"* or *"should this goal be pursued at all?"* — because the
answers live in the assumptions it is already making.

> "The person writing the code can't grade their own homework."

`[INFERENCE]` The mechanism is not dishonesty. The agent's judgment is conditioned on the
same context that produced the error. If it misread the requirement, it will also
mis-evaluate against the misreading. **A fresh context is the intervention**, not a sterner
instruction.

---

## The four levels of separation

Ranked by strength; pick the weakest one that is strong enough.

| Level | Mechanism | Independence | Cost |
|---|---|---|---|
| 0 — none | agent asserts "done" | none | free, worthless |
| 1 — deterministic gate | a **command** must exit 0 | total, but narrow | ~free |
| 2 — fresh-context judge | a separate model call, given the output and the criteria | high | one call |
| 3 — different agent, different model | reviewer subagent, ideally a different model | highest | a full run |

`[INFERENCE]` **Level 1 first, always.** A test suite is a perfect judge within its scope,
costs nothing per run, and never flatters. Reach for level 2 or 3 for what tests cannot
express — "does this match the design doc?", "is this the right abstraction?", "does the UI
actually look right?"

`[FACT]` Anthropic's stated recommendation is precisely this separation: "the solution is to
separate 'the person who does the work' from 'the person who checks the work'."

---

## Implementations

### In Claude Code

| Mechanism | Level | Notes |
|---|---|---|
| Test/lint/typecheck in a **Stop hook** | 1 | deterministic, versioned, applies to every session |
| **`/goal`** | 2 | `[FACT]` "a small fast model checks whether the condition holds" — the judge is a separate call by design |
| **Reviewer subagent** | 3 | `[FACT]` subagents "run their own loops in isolated context" — the isolation *is* the independence |
| Hook that **runs a subagent** | 3, automatic | `[FACT]` hooks can run a subagent at a lifecycle event. This is the strongest cheap option |
| **Dynamic workflow** | 3, at scale | `[FACT]` documented example: "audit a whole codebase, with a second set of agents verifying each finding" |

### In OpenAI's reported setup `[FACT]`

Codex is instructed to "review its own changes locally, request **additional specific agent
reviews** both locally and in the cloud, respond to any human or agent given feedback, and
iterate in a loop until **all agent reviewers are satisfied**"
([[Source - OpenAI Harness Engineering]]). Note that the stopping condition is *the reviewers'*
satisfaction, not the author's.

---

## The anti-pattern to check for in your own loops

```text
Who sets the "done" flag?
└─ the agent that did the work  →  you have level 0 dressed up as verification
```

`[FACT]` [[Ralph Loop]] has exactly this weakness: the implementing agent sets
`passes: true` on its own story. It is mitigated by hard quality gates ("ALL commits must
pass typecheck, lint, test") but not eliminated. If you adopt Ralph for anything that
matters, **add a checker that can flip a status back to `false`.**

`[INFERENCE]` The general test: *is there any path by which this loop can be told it was
wrong, other than the working agent noticing?* If no, the loop is a confidence amplifier.

---

## Making the evaluator good

An evaluator is only as useful as its criteria. In order of quality:

1. **A command** — `pytest -x`. Unambiguous.
2. **A checkable artefact** — a feature list with explicit steps.
   See [[Feature List as Harness Primitive]].
3. **A written rubric** — the course ships an `evaluator-rubric.md` template.
4. **"Is this good?"** — worthless. The model will say yes.

And the evaluator must be able to say **"impossible"**, not just "not yet." `[FACT]` `/goal`
has this: the loop ends if the judge "judges it impossible to satisfy." Without that state, a
maker-checker loop on an unachievable goal runs until the budget dies. See
[[Stopping Conditions]].

---

## Cost

`[INFERENCE]` A level-3 check roughly doubles the token cost of an iteration. That is the
real reason it gets skipped, and it is usually a false economy: an unverified iteration that
ships a bug costs a human review cycle, which is far more expensive than a model call. Scale
the level to the stakes — level 1 on every edit, level 3 before a merge.

---

## Related

- [[The Verification Gap]] · [[False Completion]] · [[Stopping Conditions]] · [[Feedback Quality]]
- [[Loop Types]] · [[Claude Code Graphs]] · [[Claude Code Loops]] · [[Ralph Loop]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]] · [[Source - OpenAI Harness Engineering]]
