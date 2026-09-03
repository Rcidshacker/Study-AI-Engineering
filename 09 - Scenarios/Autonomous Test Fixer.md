---
title: Autonomous Test Fixer
aliases:
  - Test fix loop
  - Scenario 4
tags:
  - scenario
  - loop-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Scenario — Autonomous Test Fixer

> [!abstract] The task
> A test suite has failures. The agent must inspect, diagnose, modify, test, analyse the new
> failure, modify again, verify, and **stop** — without supervision.

This is the best first autonomous loop to build, for one reason: **the verification signal
already exists and is perfect.** A test suite is a deterministic, independent, zero-cost judge
that never flatters. Every other loop you build afterwards will be harder because it lacks
this.

---

## Loop architecture

```text
  ┌──────────────────────────────────────────────────────────────┐
  │ 0. PRE-FLIGHT   record the baseline: which tests fail, and    │
  │                 confirm the tree is committed and clean       │
  └───────────────────────────┬──────────────────────────────────┘
                              ▼
  ┌──────────────────────────────────────────────────────────────┐
  │ 1. SELECT       pick ONE failing test                         │
  │ 2. DIAGNOSE     read the failure, the test, the code under it │
  │ 3. MODIFY       smallest change that could fix it             │
  │ 4. VERIFY       run that test, then the WHOLE suite           │
  │ 5. JUDGE                                                      │
  │      target now passes AND no new failures → COMMIT, next     │
  │      target still fails, error CHANGED    → loop to 2         │
  │      target still fails, error IDENTICAL  → stuck: revert     │
  │      other tests now fail                 → regression: revert│
  └───────────────────────────┬──────────────────────────────────┘
                              ▼
  ┌──────────────────────────────────────────────────────────────┐
  │ 6. STOP  suite green | budget spent | stuck | needs a human   │
  └──────────────────────────────────────────────────────────────┘
```

---

## The design decisions that matter

### One test at a time

`[FACT]` Directly transferred from
[[Source - Anthropic Effective Harnesses for Long-Running Agents]] — working incrementally
"turned out to be critical to addressing the agent's tendency to do too much at once."

`[INFERENCE]` With multiple simultaneous fixes you lose attribution: when the suite is still
red you cannot tell which change helped, which hurt, and which did nothing. One-at-a-time
keeps every iteration a clean experiment.

### Always run the **whole** suite

Fixing test A by breaking test B is the characteristic failure of this loop, and the agent
will not notice unless you make it look. The full run is the regression sensor.

### Commit after every green step

`[FACT]` Anthropic: commits let the model "revert bad code changes and recover working states
of the code base." `[INFERENCE]` In this loop git is not documentation — it is the **undo
stack**, and it is what makes "revert" a legal move in step 5.

### The stuck-detector is the identical error

`[INFERENCE]` This is the cheapest and most valuable heuristic in the whole loop. If the error
message is **byte-identical** after a change, the agent is not making progress — it is
re-rolling. Compare normalised failure output between iterations; on the second identical
result, revert and escalate. See [[Stopping Conditions]].

---

## The prohibition that makes it safe

> **The agent may not modify the test to make it pass.**

`[FACT]` Anthropic uses deliberately strong language for exactly this: *"It is unacceptable to
remove or edit tests because this could lead to missing or buggy functionality."*

`[INFERENCE]` Say it plainly: making tests pass is the *stated* objective, and deleting an
assertion is the globally optimal way to achieve it. If you do not close that door, a
sufficiently determined loop will eventually walk through it. Close it twice:

- **Instruction** (inferential guide) — the prohibition, in `CLAUDE.md`.
- **Mechanism** (computational guide) — a hook or pre-commit check that rejects any diff
  touching test files, or a permission rule denying writes to `tests/`.

Legitimate test changes then require you. That is correct. See
[[Executable Rules Beat Written Rules]].

---

## Stopping conditions

| State | Detected by | Action |
|---|---|---|
| Success | suite exits 0 | commit, report, exit 0 |
| Impossible | the fix needs a decision the agent cannot make | escalate with a written question |
| Budget | iteration / cost / wall-clock cap | exit **non-zero**, leave the log |
| Stuck | identical failure output twice | revert to last green, escalate |
| Regression | a previously-passing test fails | revert that change, re-select |

`[FACT]` The four-limit pattern is from
[[GitHub - SWE-agent mini-swe-agent]]: `step_limit`, `cost_limit`,
`wall_time_limit_seconds`, `max_consecutive_format_errors` — all checked *before* the model
call, all producing distinct exit statuses. Copy it.

---

## Building it in Claude Code

Three implementations, ascending in autonomy.

**A — Stop hook (start here).** A script that runs the suite after every turn and, on failure,
returns the failure output. The loop continues while the script is unhappy. Deterministic
judge, versioned in settings, applies to every session. See [[Claude Code Hooks]].

**B — `/goal`.** `/goal "the full test suite passes and no test file has been modified"`.
`[FACT]` A separate fast model checks the condition after each turn, and can also terminate by
judging it impossible. Fastest to try; session-scoped. See [[Claude Code Loops]].

**C — External loop (most control).** Headless Claude Code inside a shell loop, in the
[[Ralph Loop]] shape: fresh context each iteration, state in a status file plus git, hard
iteration cap, non-zero exit on exhaustion. Use this when you want to run it unattended,
overnight, in a container.

Whichever you pick, the loop is only as good as its inputs:

| Requirement | Note |
|---|---|
| A single command that runs the suite and exits meaningfully | in `CLAUDE.md` |
| Failure output that names file, line and expectation | [[Feedback Quality]] |
| A clean, committed tree before starting | [[Clean State Ritual]] |
| Isolation for unattended runs | [[Worktree Isolation]], [[Sandboxing and Permissions]] |

---

## Why this scenario generalises

`[INFERENCE]` Every reliable autonomous loop has the same five parts, and here each is
unusually easy to see:

| Part | Here |
|---|---|
| Machine-checkable done | the suite exits 0 |
| Independent judge | the test runner — it did not write the code |
| Bounded unit of work | one failing test |
| Recoverable state | git commit per green step |
| Real stopping conditions | success, stuck, regression, budget, escalation |

Build this first. Then, when you move to a task without a natural judge, you will feel exactly
what is missing — and that feeling is [[The Verification Gap]].

---

## Related

- [[Loop Engineering]] · [[Stopping Conditions]] · [[Generator Evaluator Separation]]
- [[Ralph Loop]] · [[Claude Code Loops]] · [[The Verification Gap]] · [[False Completion]]
- [[Scenarios MOC]]
