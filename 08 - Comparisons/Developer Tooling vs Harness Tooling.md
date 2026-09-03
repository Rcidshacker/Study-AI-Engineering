---
title: Developer Tooling vs Harness Tooling
aliases:
  - Is this just developer tooling
  - Claude Code vs Agent Frameworks
tags:
  - comparison
  - harness-engineering
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Developer Tooling vs Harness Tooling

> [!abstract] One line
> Same artefacts, different reader, and one decisive difference: the thing being wrapped is
> **non-deterministic**, so the tooling must be designed to recover from confident wrongness.

---

## The reasonable objection

*Tests, linters, CI, docs, reproducible environments — this is all just good engineering
practice with a new name.*

`[INFERENCE]` **Largely yes, and that is a point in its favour.** Most of what makes an agent
reliable is what makes a codebase good. If the discipline required entirely novel machinery
it would be far more suspicious. But three things genuinely differ.

---

## Difference 1 — the wrapped component is non-deterministic `[FACT]`

[[Source - Wikipedia Agent Harness]] names this as *the* distinguishing feature:

> "A distinguishing feature is that the component being wrapped is **non-deterministic**, so a
> harness is designed to **recover gracefully when the model fabricates an action or reports a
> task as finished when it is not.**"

`[INFERENCE]` Ordinary developer tooling assumes the human eventually notices. Its failure
model is *the developer is wrong sometimes* and its remedy is *tell them*. Harness tooling
assumes the reader is fluent, confident, tireless, and occasionally wrong in ways that are
indistinguishable from right — and that nobody may be watching. That changes what you build:
stopping conditions, budgets, independent judges, immutable definitions of done. None of those
appear in a normal toolchain.

---

## Difference 2 — the reader reasons, so text is an interface `[FACT]`

Both OpenAI and Thoughtworks independently write **remediation instructions into their linter
messages**. Anthropic names "carefully craft your **agent-computer interface** through thorough
tool documentation and testing" as a core principle, and titles an appendix *"Prompt
Engineering your Tools."*

`[INFERENCE]` This is the genuinely new engineering surface. In human tooling, a good error
message is a courtesy. In agent tooling it is a **control** — a guide delivered by a sensor at
the moment of the mistake, at zero cost when nothing is wrong. Prose quality inside your
toolchain became a functional property. See [[Feedback Quality]].

---

## Difference 3 — throughput can exceed review capacity `[FACT]`

> "In a system where agent throughput far exceeds human attention, corrections are cheap, and
> waiting is expensive. **This would be irresponsible in a low-throughput environment.**"

`[INFERENCE]` Human-scale tooling assumes a review bottleneck at the *end*. When production
outpaces review, the checks must move **into** the loop, cleanup must be **continuous**
rather than periodic, and merge policy has to change. That is a different design point, not a
different toolbox. See [[Harness Debt and Garbage Collection]].

---

## Side by side

| | Developer tooling | Harness tooling |
|---|---|---|
| Reader | a human who asks questions | a model that cannot |
| On ambiguity | asks a colleague | guesses, confidently |
| Error message | a courtesy | **a control** |
| Missing context | someone remembers | it does not exist |
| Failure model | occasionally wrong, notices | confidently wrong, may not |
| Review | at the end | inside the loop |
| Rules | conventions people follow | mechanically enforced or ignored |
| Cleanup | periodic | **continuous** |

---

## What this means practically `[INFERENCE]`

- **Most of your existing toolchain is reusable**, and better used than replaced. A test suite
  is a perfect judge; a type checker is free continuous feedback.
- **What it lacks is the recovery layer**: budgets, stop conditions, an independent checker, an
  immutable definition of done.
- **The cheapest upgrade is not new tooling** — it is making existing tool *output* legible to
  a reasoning reader.

---

## The related question: framework or tool? `[INFERENCE]`

The same instinct applies to *"should I use an agent framework?"* Anthropic's answer is
unusually direct `[FACT]`: "start by using LLM APIs directly: many patterns can be implemented
in a few lines of code. If you do use a framework, ensure you understand the underlying code.
Incorrect assumptions about what's under the hood are a common source of customer error." And
frameworks "often create extra layers of abstraction that can obscure the underlying prompts
and responses."

The vault's position: a coding agent like [[Claude Code]] gives you an inner harness plus
extension points; a framework gives you construction materials. If you are *using* an agent on
your codebase, your work is the **outer harness** — instruction files, tests, hooks,
permissions — and that work is largely portable between agents. See
[[Inner Harness vs Outer Harness]].

---

## Related

- [[Harness Engineering]] · [[Feedback Quality]] · [[Executable Rules Beat Written Rules]]
- [[Inner Harness vs Outer Harness]] · [[Harness Debt and Garbage Collection]]
- [[Source - Wikipedia Agent Harness]] · [[Source - Anthropic Building Effective Agents]]
