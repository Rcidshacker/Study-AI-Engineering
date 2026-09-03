---
title: Harness Ablation Testing
aliases:
  - Harnessability
  - Measuring a harness
  - Agent Evaluation
tags:
  - harness-engineering
  - evaluation
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Harness Ablation Testing

> [!abstract] One line
> Hold the model fixed, remove one control at a time, measure the drop. It tells you what is
> earning its keep — and, importantly, it does **not** tell you where your bottleneck is.

---

## The method `[FACT]`

> "Keep the model fixed, remove the five subsystems one at a time, and see which subsystem's
> removal causes the biggest performance drop."

`[FACT]` Anthropic is reported to have used this and found something worth planning for:

> "as models get stronger, **some components stop being critical** — but new critical components
> always emerge."

`[INFERENCE]` So a harness has a **shelf life**. Controls compensating for model weaknesses
become pure context cost when the weakness disappears. Re-run this after every model upgrade;
it is the only reliable way to find dead weight, and dead weight is not free —
see [[Context Window as a Budget]].

---

## The limit, stated by the source itself `[FACT]`

> "This experiment answers 'which component is most valuable right now' — **it cannot, by
> itself, prove 'where the bottleneck is.'** To truly locate a bottleneck, you must first
> examine failure records and attributions… Component ablation results can only serve as
> supporting evidence."

And on near-zero results:

> "Components with near-zero impact should not be dismissed outright: they may be redundant,
> poorly designed, or **simply not exercised by the current task**."

`[INFERENCE]` This is the trap. A low ablation score has at least four readings — redundant,
badly built, untested by this task, or genuinely unnecessary — and only the last justifies
deletion. Pair every ablation with failure logs.

---

## Running one

1. **Fix everything else.** One model, one task set, one prompt.
2. **Use several tasks.** `[INFERENCE]` A single task exercises a subset of your controls, so a
   single-task ablation mostly measures which subsystem that task happened to need.
3. **Remove one subsystem**: delete the instruction file; withhold verification commands;
   remove the progress file; break reproducibility; restrict tools.
4. **Measure something you can count** — pass rate over N runs, iterations to done, cost.
5. **Record the failure mode too**, not just the number. *How* it failed is the diagnostic part.

---

## Harnessability — the other half `[FACT]`

Böckeler names a property of the **codebase**, not the agent: how amenable your system is to
being regulated at all.

`[INFERENCE]` A codebase with no tests, no types, and no module boundaries gives computational
sensors nothing to grip. You can write instructions and run an LLM judge, but the cheap,
deterministic half of [[Guides and Sensors]] has no attachment points.

| Harnessability signal | Enables |
|---|---|
| A test suite that fails when behaviour breaks | the whole verification layer |
| Static types | continuous, free feedback via code intelligence |
| Enforced module boundaries | structural tests — [[Executable Rules Beat Written Rules]] |
| One command to build and run | `init.sh`, loops, CI |
| Reproducible environment | isolation, parallelism |

**If harnessability is low, that is the work** — and it is work worth doing regardless of
agents. See [[When Not to Build a Harness]].

---

## Evaluating agents, not just harnesses `[FACT]`

The public reference point is **SWE-bench** — "Can Language Models Resolve Real-world Github
Issues?" (`SWE-bench/SWE-bench`, 5,769★, MIT, verified 2026-09-04) — and
[[GitHub - SWE-agent mini-swe-agent]], which reports >74% on SWE-bench Verified.

`[INFERENCE]` Two cautions when reading any such number:

1. **A benchmark measures a model *through a harness*.** Change either and the number moves.
   That is exactly why a ~100-line agent can be competitive: much of what is measured is
   scaffolding quality. See [[Harness Beats Model Choice]].
2. **Your task is not the benchmark.** The transferable practice is not the score — it is
   building a small, repeatable task set for *your* codebase, so a harness change has a number
   attached rather than an impression.

---

## The minimum viable evaluation `[INFERENCE]`

You do not need an eval framework to start:

- Five to ten real tasks from your own backlog, with known-good outcomes.
- A script that runs each N times and records pass/fail, iterations, and cost.
- Run it before and after any harness change worth making.

That is enough to stop guessing, and it is the difference between engineering a harness and
decorating one.

---

## Related

- [[Harness Components]] · [[Guides and Sensors]] · [[Harness Beats Model Choice]]
- [[When Not to Build a Harness]] · [[Harness Debt and Garbage Collection]]
- [[Source - Learn Harness Engineering Course]] · [[Source - Harness Engineering for Coding Agent Users]]
