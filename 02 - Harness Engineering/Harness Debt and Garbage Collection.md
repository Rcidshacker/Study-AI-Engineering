---
title: Harness Debt and Garbage Collection
aliases:
  - Continuous Drift Sensors
  - AI slop cleanup
  - Harness rot
tags:
  - harness-engineering
  - production
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Harness Debt and Garbage Collection

> [!abstract] One line
> Agents replicate the patterns already in your repository, including the bad ones. Drift is
> not a risk to manage; it is a **certainty to service**.

---

## The mechanism `[FACT]`

> "Codex replicates patterns that already exist in the repository — even uneven or suboptimal
> ones. Over time, this inevitably leads to drift."

`[INFERENCE]` This is a **positive feedback loop**, which is why it does not stay small: one
mediocre pattern gets copied, becomes the dominant local convention, and is then copied more
confidently. Human codebases drift too, but slowly and with someone occasionally objecting.
Agent throughput removes both brakes.

---

## The failed fix, reported honestly `[FACT]`

> "Initially, humans addressed this manually. Our team used to spend **every Friday (20% of the
> week)** cleaning up 'AI slop.' Unsurprisingly, that didn't scale."

`[INFERENCE]` Twenty percent of a team's time is the price of doing this manually at high
throughput, and it does not hold as throughput rises. Anyone planning agent-heavy development
should budget for cleanup from day one, and plan for it to be automated rather than staffed.

---

## The working fix `[FACT]`

Two parts:

1. **Golden principles** — "opinionated, mechanical rules that keep the codebase legible and
   consistent for future agent runs," encoded in the repository. Two published examples:
   - prefer shared utility packages over hand-rolled helpers, to keep invariants centralised
   - do not probe data "YOLO-style" — validate boundaries or use typed SDKs, so the agent
     cannot build on guessed shapes
2. **A recurring cleanup loop** — "a set of background Codex tasks that scan for deviations,
   update quality grades, and open targeted refactoring pull requests. Most of these can be
   reviewed in under a minute and automerged."

> "This functions like **garbage collection**. Technical debt is like a high-interest loan:
> it's almost always better to pay it down continuously in small increments than to let it
> compound."

`[INFERENCE]` The reviewability constraint is doing quiet work here. Cleanup PRs are scoped to
be readable in under a minute *by design* — which is what makes automerging them defensible.
A large refactoring PR from an agent is not garbage collection; it is a new risk.

---

## Continuous drift sensors `[FACT]`

Böckeler's timing framework names this as its own category, distinct from the change lifecycle:

> "What type of drift accumulates gradually and should be monitored by sensors running
> **continuously against the codebase, outside the change lifecycle**? (e.g. dead code
> detection, analysis of the quality of the test coverage, dependency scanners)"

And the runtime tier above it:

> "What runtime feedback could agents be monitoring? (e.g. having them look for degrading SLOs
> to make suggestions how to improve them, or AI judges continuously sampling response quality
> and flagging log anomalies)"

`[INFERENCE]` These two rows are what almost every hand-built setup omits, and they are the
only ones that catch **drift** rather than **defects**. A per-commit check cannot see a trend;
that is not what it is for.

| Sensor | Detects | Cadence |
|---|---|---|
| Dead-code detection | accumulation | weekly |
| Duplicate-code scan | hand-rolled helpers replacing shared ones | weekly |
| Coverage *quality* analysis | tests that assert nothing | weekly |
| Dependency scan | staleness, vulnerabilities | daily |
| Architecture conformance | boundary erosion | per commit **and** as a trend |
| SLO watch | runtime regression | continuous |

---

## Two kinds of debt

`[INFERENCE]` Worth separating, because they are serviced differently:

| | **Code debt** | **Harness debt** |
|---|---|---|
| What rots | the codebase | the controls themselves |
| Symptoms | duplication, drift, dead code | stale rules, checks nobody reads, controls for problems that no longer exist |
| Serviced by | cleanup loops | periodic audit |

`[FACT]` Harness debt is real and named: a monolithic instruction file "turns into a graveyard
of stale rules… humans stop maintaining it, and the file quietly becomes an attractive
nuisance." And: "as models get stronger, **some components stop being critical** — but new
critical components always emerge."

`[INFERENCE]` So the harness needs the same treatment as the code: an owner, a review cadence,
and a debt tracker. **Re-run [[Harness Ablation Testing]] after every model upgrade** — a
control that was load-bearing last year may now be pure context cost.

---

## Doing it yourself

1. Write down your golden principles. Three to five, mechanical, opinionated.
2. Make each one a **check** where possible — see [[Executable Rules Beat Written Rules]].
3. Schedule a drift scan. `[FACT]` Claude Code documents scheduled tasks and routines for
   exactly this shape of work; the loop type is time-based — see [[Loop Types]].
4. Constrain cleanup PRs to be reviewable in a minute.
5. Audit the harness itself on the same cadence as you audit the code.

---

## Related

- [[Instruction File Design]] · [[Executable Rules Beat Written Rules]] · [[Guides and Sensors]]
- [[Harness Ablation Testing]] · [[Production Coding Agent]] · [[Loop Types]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Harness Engineering for Coding Agent Users]]
