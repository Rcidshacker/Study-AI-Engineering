---
title: Harness Failure Modes
aliases:
  - Loop Failure Modes
  - Failure taxonomy
  - Retry Strategies
tags:
  - harness-engineering
  - loop-engineering
  - failure-mode
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Harness and Loop Failure Modes

> [!abstract] One line
> Every documented failure I found reduces to one of nine, and each has a cheapest control.
> Diagnose before building.

---

## Layer 1 — failures of the substrate

| # | Failure | Root cause | Cheapest control |
|---|---|---|---|
| 1 | Wrong conventions, wrong package manager, reinvented helpers | it was never told | a short, specific instruction file — [[Instruction File Design]] |
| 2 | Stops and asks you to run things | capability missing | grant the tool; least privilege, not no privilege |
| 3 | "Works on my machine"; parallel runs collide | environment not reproducible or not isolated | `init.sh`, lockfiles, [[Worktree Isolation]] |
| 4 | Re-solves solved problems; loses the thread | state lives in the transcript | [[External State]] |
| 5 | **Ships broken code confidently** | **cannot observe consequences** | **a real end-to-end check** — [[The Verification Gap]] |
| 6 | Edits the wrong files | scope was a sentence, not a boundary | [[Sandboxing and Permissions]] |

`[INFERENCE]` Row 5 is the majority case and the one most often misdiagnosed as "the model
isn't good enough." Run the diagnostic in [[The Verification Gap]] before changing models.

---

## Layer 2 — failures of the loop

| # | Failure | Root cause | Control |
|---|---|---|---|
| 7 | **Declares victory early** | no durable definition of done; author grades itself | [[Feature List as Harness Primitive]] + [[Generator Evaluator Separation]] |
| 8 | **Never stops** | only one terminal state | five terminal states — [[Stopping Conditions]] |
| 9 | **Stuck: iterating without progressing** | no stuck-detector | see below |

### The two named long-running failures `[FACT]`

**One-shotting** — "the agent tended to try to do too much at once… running out of context in
the middle of its implementation, leaving the next session to start with a feature
half-implemented and undocumented."
**Fix:** one feature per session. "This incremental approach turned out to be critical."

**Premature victory** — "a later agent instance would look around, see that progress had been
made, and declare the job done."
**Fix:** the feature list. See [[False Completion]].

---

## Detecting "stuck"

`[INFERENCE]` A stuck loop is worse than a failed one: it burns budget while producing a
plausible transcript. Cheap heuristics, in order of cost:

| Signal | Implementation |
|---|---|
| **N consecutive malformed outputs** | `[FACT]` `mini-swe-agent`'s `max_consecutive_format_errors`, default 3 — and it **resets on any clean step**, so it detects *being stuck now*, not *having ever failed* |
| No file changed in N iterations | it is thinking, not working |
| Identical error output twice | `[INFERENCE]` the best single heuristic — re-rolling, not diagnosing |
| The diff oscillating | iteration *n* reverts *n−1*: a requirement conflict it cannot see |
| Progress entries repeating | visible in [[Ralph Loop]]-style logs |

---

## Retry strategy

`[INFERENCE]` Retrying is only useful if **something changed**. Classify before retrying:

| Failure kind | Retry? | Why |
|---|---|---|
| Transient (network, flake) | yes, with backoff | `[FACT]` OpenAI handles test flakes "with follow-up runs rather than blocking progress indefinitely" |
| Malformed output | yes, up to a small cap | often recovers on the next sample |
| Same error, same approach | **no** | nothing changed; escalate or revert |
| Verification failed with *new* information | **yes** — this is the loop working | the failure is the signal |
| Blocked on a decision | **no** | escalate with a written question |

`[FACT]` `/goal` clears when a turn fails on "an error you have to fix," and can also terminate
by judging the goal **impossible**. `[INFERENCE]` Both are retry-suppression mechanisms, and
they are what stop a maker-checker loop grinding on an unachievable target.

---

## The four silent costs of running loops at all `[FACT — reported via Osmani]`

Not bugs; consequences that sharpen the longer a loop runs:

1. **Verification debt** — output outpacing confirmation
2. **Comprehension rot** — a codebase nobody has read
3. **Cognitive surrender** — approving because checking is tiring
4. **Token blowout** — cost scaling with iterations, not value

`[INFERENCE]` 1 and 3 are the same failure at different layers, and together they are why
[[Generator Evaluator Separation]] is non-negotiable: if the only reviewer is a tired human,
effective verification approaches zero as throughput rises. 2 is what
[[Harness Debt and Garbage Collection]] exists to service.

---

## The diagnostic order

Cheapest first. `[INFERENCE]` Most teams start at step 5 and should start at step 1.

```text
1. Was the instruction ambiguous?              → prompt
2. Did it have the information?                → context
3. Could it see it was wrong?                  → feedback   ← usually here
4. Did it have the tool / the environment?     → tools, environment
5. Did it stop wrongly, or never start?        → loop
6. Did the wrong specialist take the work?     → graph
```

---

## Related

- [[The Verification Gap]] · [[False Completion]] · [[Stopping Conditions]] · [[Generator Evaluator Separation]]
- [[Harness Components]] · [[Harness Architecture]] · [[The Unified Mental Model]]
- [[Autonomous Test Fixer]] · [[Scenarios MOC]]
