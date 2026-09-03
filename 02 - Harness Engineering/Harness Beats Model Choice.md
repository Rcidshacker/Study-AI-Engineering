---
title: Harness Beats Model Choice
aliases:
  - Harness over model
  - Does a better model remove the need for a harness
tags:
  - harness-engineering
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Harness Beats Model Choice

> [!abstract] One line
> On real multi-step work, the environment around the model explains more of the outcome
> variance than the choice of model — and the strongest evidence for this comes from the
> model vendors themselves.

> [!warning] Read the confidence rating
> This is the vault's most useful claim and also its least rigorously evidenced. There is **no
> controlled study** here. What exists is: one detailed first-party engineering report, one
> first-party negative result, and one unsourced case study. That is enough to act on and not
> enough to quote as a measured fact. Marked accordingly throughout.

---

## The evidence, strongest first

### 1. A frontier lab's negative result `[FACT]`

[[Source - Anthropic Effective Harnesses for Long-Running Agents]]:

> "Out of the box, even a frontier coding model like **Opus 4.5** running on the Claude Agent
> SDK in a loop across multiple context windows **will fall short** of building a
> production-quality web app if it's only given a high-level prompt."

This is the cleanest possible form of the argument: *the people who make the model say the
model alone is not enough*, and they name the model. It does not quantify the harness's
contribution, but it removes the "just wait for a better model" position.

### 2. A frontier lab's positive result `[FACT]`

[[Source - OpenAI Harness Engineering]]: ~1M lines, ~1,500 merged PRs, 3→7 engineers, 3.5
PRs/engineer/day, ~1/10th the time, zero hand-written lines — over five months **during which
the model was not the variable being changed.** What changed continuously was the harness:
repo structure, custom linters, worktree-bootable app, per-worktree observability, golden
principles, garbage-collection loops.

`[INFERENCE]` It is a case study, not an experiment; there is no counterfactual arm. But the
*narrative* of the post is explicitly about harness investment producing the result, and the
authors are the ones with the most to gain from crediting the model instead.

### 3. The staged case study `[UNVERIFIED]`

[[Source - Learn Harness Engineering Course]] reports a team on a ~20k-line TypeScript/React
app with GPT-4o:

| Stage | Added | Success rate |
|---|---|---|
| 1 | README only | 20% |
| 2 | `AGENTS.md` — stack, conventions, architecture | 60% |
| 3 | verification commands in `AGENTS.md` | 80% |
| 4 | progress-file templates | 80–100% |

> "The model did not change at all, and success rate went from 20% to near 100%."

**No team, publication or dataset is named, and n=5 per stage.** `[INFERENCE]` Treat this as
an *illustration of the shape* — most of the gain arriving from instructions and verification
— and not as a measurement. The shape is corroborated by sources 1 and 2; the magnitudes are
not corroborated by anything.

### 4. Reported research `[UNVERIFIED]`

[[Source - Wikipedia Agent Harness]] describes **Harness-1**, "an open-source search agent
that improved retrieval accuracy chiefly by redesigning the software environment around the
model rather than by enlarging the model." I could not locate the paper. If it exists and
holds up, it is the controlled evidence this claim currently lacks — worth chasing.

---

## Why it is true, mechanically `[INFERENCE]`

The argument does not need statistics; it follows from what a model is.

1. The model is **stateless**. On a multi-session task, *all* continuity is supplied by the
   environment. A better model does not remember more; it has nothing to remember with.
2. The model **only emits text**. Every capability — running a test, seeing a log, driving a
   browser — is granted by the harness. A better model cannot grant itself a tool.
3. The model **cannot see consequences** it is not shown. Model quality improves the *guess*;
   only feedback converts the guess into knowledge. See [[The Verification Gap]].

So a stronger model raises the quality of each step, while the harness determines **how many
steps are possible, whether errors are caught, and whether progress survives**. Those are
different axes, and on long tasks the second dominates — one uncaught error early can waste
every subsequent step.

---

## The honest counter-arguments

`[FACT]` The course reports that ablation testing revealed "as models get stronger, **some
components stop being critical** — but new critical components always emerge."

`[INFERENCE]` Three real limits on this claim:

- **Some harness components are compensating for model weaknesses** that will disappear. Elaborate
  prompt scaffolding for planning is the obvious example. Building against a weakness is
  building a depreciating asset.
- **Model choice matters enormously for the inferential half** of your harness. An LLM-judge,
  a reviewer subagent, or `/goal`'s completion check is only as good as the model running it.
  The harness does not make a weak judge strong.
- **A harness cannot exceed the model's ceiling on a single step.** It can catch a wrong step
  and force a retry; it cannot make the model capable of a step it cannot do at all.

The accurate formulation is therefore narrower than the note title: **the harness dominates on
long, multi-step, unattended work; the model dominates on single hard steps.**

---

## What follows practically

- Before switching models to fix a failure, run the diagnostic in [[The Verification Gap]].
  If nothing in the environment could have told the agent it was wrong, the model was never
  the variable.
- Prefer **outer-harness** investment: it transfers when you change models or tools, and
  models change more often than repositories do. See [[Inner Harness vs Outer Harness]].
- Prefer **computational** controls where possible: they are model-independent by construction.
  See [[Guides and Sensors]].
- Re-run [[Harness Ablation Testing]] after a model upgrade. Components that were load-bearing
  may have become dead weight, and dead weight costs context.

---

## Related

- [[The Verification Gap]] · [[Harness Components]] · [[Harness Ablation Testing]]
- [[Inner Harness vs Outer Harness]] · [[When Not to Build a Harness]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]] · [[Source - OpenAI Harness Engineering]]
