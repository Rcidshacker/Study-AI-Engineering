---
title: Stopping Conditions
aliases:
  - Stop conditions
  - Retry limits
  - Loop budgets
tags:
  - loop-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Stopping Conditions

> [!abstract] One line
> A loop needs **at least three** ways to end — success, impossibility, and budget — and it
> must report which one happened. A loop with only a success condition is an infinite loop
> with optimism.

---

## The five terminal states

Every well-built loop I have read implements some subset of these. Aim for all five.

| # | State | Meaning | Detected by |
|---|---|---|---|
| 1 | **Success** | the goal condition holds | an independent evaluator — [[Generator Evaluator Separation]] |
| 2 | **Impossible** | the goal cannot be satisfied | judge verdict, or a blocking error |
| 3 | **Budget exhausted** | out of iterations, money, or time | counters checked before each call |
| 4 | **Stuck** | still running, but not progressing | repetition / no-change detection |
| 5 | **Escalation** | needs a human | explicit signal from the agent |

`[INFERENCE]` States 2, 4 and 5 are the ones hand-rolled loops omit, and they are the ones
that make unattended running safe. A loop with only states 1 and 3 will burn its whole budget
on an impossible task and then report "budget exhausted," which tells you nothing about why.

---

## Worked example: mini-swe-agent `[FACT — read from source 2026-09-04]`

The cleanest implementation of budgets I have found. `AgentConfig` declares four independent
limits:

| Field | Default | Guards against |
|---|---|---|
| `step_limit` | 0 (off) | endless iteration |
| `cost_limit` | **3.0** | runaway spend |
| `wall_time_limit_seconds` | 0 (off) | hanging |
| `max_consecutive_format_errors` | **3** | a model stuck emitting unparseable output |

Three design details worth copying verbatim:

1. **Limits are checked *before* the model call**, not after. Enforcement at the boundary.
2. **The format-error counter is `n_consecutive` and resets on any clean step.** It detects
   *being stuck now*, not *having ever failed*. A cumulative counter would kill healthy long
   runs — this is the difference between a stuck-detector and a flakiness-detector.
3. **Every exit path produces a distinct status** — `LimitsExceeded`, `TimeExceeded`,
   `RepeatedFormatError`, or the exception class name — funnelled through one uniform exit
   channel (a message with `role == "exit"`).

See [[GitHub - SWE-agent mini-swe-agent]].

## Worked example: Ralph `[FACT]`

`MAX_ITERATIONS` defaults to 10. Success is a sentinel string (`<promise>COMPLETE</promise>`)
emitted only when every item in `prd.json` has `passes: true`. **Exhausting the budget exits
non-zero** — running out of iterations is reported as a failure, not as completion. See
[[Ralph Loop]].

## Worked example: `/goal` `[FACT]`

Per the official docs, `/goal` clears when: a model confirms the condition is met, **or judges
it impossible**, or a turn fails on an error you have to fix, or you run `/goal clear`. That
is states 1, 2, 5 and manual override — with the notable inclusion of state 2, which most
hand-built loops lack. See [[Claude Code Loops]].

---

## Writing a success condition

The condition must be **evaluable by something other than the agent's opinion**. Ranked:

| Quality | Form | Example |
|---|---|---|
| best | a command's exit code | `pytest -x && ruff check src/` |
| good | a checkable artefact | every entry in `feature_list.json` has `passes: true` |
| workable | a rubric an independent judge applies | `evaluator-rubric.md` |
| useless | a description of quality | "until the code is good" |

`[INFERENCE]` A useful test before starting any loop: **write down the shell command that
proves you are done.** If you cannot, you are not ready to loop — you are ready to define
done. See [[Feature List as Harness Primitive]].

---

## Detecting "stuck"

Cheap heuristics, roughly in order of cost:

- **N consecutive malformed outputs** — mini-swe-agent's approach, defaults to 3.
- **No file changed in the last N iterations** — the loop is thinking, not working.
- **The same command failing with the same error N times** — retrying without changing anything.
- **The diff oscillating** — iteration *n* reverts iteration *n−1*. A strong signal of a
  requirement conflict the agent cannot see.
- **Progress-file entries repeating themselves** — visible in [[Ralph Loop]]-style logs.

`[INFERENCE]` A stuck loop is worse than a failed one, because it consumes budget while
producing a plausible-looking transcript. Detecting it is worth more than another retry.

---

## Budgets are a safety mechanism, not a cost control

`[INFERENCE]` It is tempting to set generous limits so runs are not cut short. That inverts
the purpose. The budget is what bounds the blast radius of a loop that has gone wrong in a way
you did not anticipate — and by construction, you did not anticipate it. Set the budget to
what a *successful* run should cost plus a margin, and treat hitting it as a signal to
investigate rather than a number to raise.

Start every new loop with a **small** budget and read the log after every run. Raise it only
once you have seen the loop succeed.

---

## The reporting rule

Whatever ends the loop, the loop must say **which** state it ended in, and leave that in a
durable artefact — an exit code, a status field, a progress-file entry. `[INFERENCE]` The
commonest silent failure in unattended automation is a loop that stops for reason 3 and is
read by a human as reason 1.

---

## Related

- [[Loop Types]] · [[Loop Engineering]] · [[Generator Evaluator Separation]] · [[Loop Failure Modes]]
- [[Retry Strategies]] · [[Ralph Loop]] · [[Claude Code Loops]]
- [[GitHub - SWE-agent mini-swe-agent]]
