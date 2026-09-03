---
title: Guides and Sensors
aliases:
  - Feedforward and feedback controls
  - Computational vs Inferential
tags:
  - harness-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Guides and Sensors

> [!abstract] One line
> Every harness control is one of four things: it either **steers before** the agent acts or
> **observes after**, and it is either **deterministic** or **model-judged**. Build in all
> four quadrants or you get a predictable pathology.

`[FACT]` The framework is Birgitta Böckeler's, in
[[Source - Harness Engineering for Coding Agent Users]] (Thoughtworks, 2 April 2026). It is
the most useful organising device in the harness literature because it is **complete** —
every control you can name lands in exactly one cell.

---

## The two axes

**Direction — when does it act?**

- **Guides (feedforward)** — anticipate the agent's behaviour and steer it *before* it acts.
  Raise the probability of a good result on the first attempt.
- **Sensors (feedback)** — observe *after* the agent acts and let it self-correct.

**Execution type — what runs it?**

- **Computational** — deterministic, CPU, milliseconds to seconds, reliable results.
- **Inferential** — semantic, GPU/NPU, slower and more expensive, non-deterministic results.

---

## The 2×2, with Claude Code implementations

|  | **Computational** (deterministic) | **Inferential** (model-judged) |
|---|---|---|
| **Guide** (before) | type system · lockfiles · scaffolding scripts · code mods (OpenRewrite) · templates · `init.sh` | `CLAUDE.md` conventions · skills · plan mode · few-shot examples in a skill body |
| **Sensor** (after) | tests · linters · type checker · structural/architecture tests (ArchUnit) · build · coverage gates | subagent code review · LLM-as-judge rubric · "does this satisfy the acceptance criteria?" |

`[FACT]` Böckeler's own published examples: coding conventions = inferential guide
(`AGENTS.md`, Skills); project bootstrap = both (a skill *and* a script); code mods =
computational guide; structural tests = computational sensor (pre-commit hook running
ArchUnit); how-to-review instructions = inferential sensor (Skills).

---

## The failure modes of an incomplete grid `[FACT]`

> "Separately, you get either an agent that keeps repeating the same mistakes
> (feedback-only) or an agent that encodes rules but never finds out whether they worked
> (feed-forward-only)."

Extending that along the other axis `[INFERENCE]`:

| What you have | What goes wrong |
|---|---|
| Guides only | The agent repeats mistakes forever. Rules exist; nothing checks them. |
| Sensors only | The agent rediscovers your conventions by trial and error, burning tokens each time. |
| Computational only | Catches structure — duplication, complexity, coverage, boundaries — and is blind to *"this is the wrong solution to the right problem."* |
| Inferential only | Slow, expensive, non-deterministic, and you will not trust it enough to act on it unattended. |

**Diagnosis heuristic**: an agent that keeps making the *same* mistake is missing a guide.
An agent that makes *confident but wrong* claims is missing a sensor.

---

## Sensors should speak to the model `[FACT]`

The highest-leverage detail in the whole framework:

> Sensors are "particularly powerful when they produce signals that are optimised for LLM
> consumption, e.g. custom linter messages that include instructions for the self-correction —
> **a positive kind of prompt injection.**"

> [!important] Independent convergence
> [[Source - OpenAI Harness Engineering]] reports the identical practice, arrived at
> independently two months earlier: "because the lints are custom, we write the error
> messages to inject remediation instructions into agent context."
>
> Two separate organisations landing on the same non-obvious technique is the strongest
> evidence in this vault that a practice is real rather than fashionable.

Practical form:

```text
BAD   error: no-any at src/api.ts:42
GOOD  error: no-any at src/api.ts:42 — this project parses external data at the
      boundary. Define a zod schema in src/schemas/ and parse there; see
      docs/design-docs/core-beliefs.md#boundary-parsing for the pattern.
```

The second version is a **guide delivered through a sensor**. That is the whole trick. See
[[Feedback Quality]] and [[Executable Rules Beat Written Rules]].

---

## Timing: keep quality left `[FACT]`

Distribute controls by cost, speed and criticality:

| When | What belongs there |
|---|---|
| Before commit / pre-integration | linters, fast tests, a basic review agent |
| Post-integration pipeline | mutation testing, broad review that sees the bigger picture |
| Continuously, outside the change lifecycle | dead-code detection, coverage-quality analysis, dependency scanners |
| At runtime | agents watching degrading SLOs; AI judges sampling response quality, flagging log anomalies |

`[INFERENCE]` The bottom two rows are what almost every hand-rolled harness omits, and they
are the ones that catch **drift** rather than defects. See [[Continuous Drift Sensors]].

---

## The steering loop `[FACT]`

> "The human's job in this is to steer the agent by iterating on the harness. Whenever an
> issue happens multiple times, the feedforward and feedback controls should be improved to
> make the issue less probable to occur in the future, or even prevent it."

The trigger is **repetition**, not severity. One-off mistakes get corrected in the chat;
repeated mistakes get a control. See [[Fix the Class Not the Instance]].

And the controls are now cheap to build, because the agent builds them: it "can help write
structural tests, generate draft rules from observed patterns, scaffold custom linters, or
create how-to guides from codebase archaeology."

---

## Harness categories `[FACT]`

Böckeler argues "harness" is too generic and qualifies it by what is being regulated:

1. **Maintainability harness** — internal code quality. Easiest today: decades of existing
   tooling apply, and computational sensors reliably catch duplication, complexity, missing
   coverage, architectural drift and style.
2. **Architecture fitness harness** — conformance to the intended architecture.
3. **Behaviour harness** — whether the software does the right thing. Hardest.

`[INFERENCE]` The difficulty ordering is also the *coverage* ordering in practice: most teams
have a decent maintainability harness by accident (they already had linters), a weak
architecture harness, and essentially no behaviour harness. That is exactly why
[[The Verification Gap]] is where agents fail.

---

## Related

- [[Harness Components]] · [[Feedback Quality]] · [[The Verification Gap]] · [[Harnessability]]
- [[Inner Harness vs Outer Harness]] · [[Generator Evaluator Separation]]
- [[Source - Harness Engineering for Coding Agent Users]]
