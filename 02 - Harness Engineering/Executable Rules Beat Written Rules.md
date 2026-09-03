---
title: Executable Rules Beat Written Rules
aliases:
  - Promote the rule into code
  - Enforce invariants
tags:
  - harness-engineering
  - practice
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Executable Rules Beat Written Rules

> [!abstract] One line
> A rule in prose is advisory and costs context every session. The same rule as a check is
> enforced and costs nothing until it fires.

---

## The claim `[FACT]`

[[Source - OpenAI Harness Engineering]] states both halves.

The principle:

> "**Enforce invariants, don't micromanage implementation.**"

Worked example: they require Codex to parse data shapes at the boundary but do **not**
prescribe the library. "The model seems to like Zod, but we didn't specify that specific
library."

The mechanism:

> "These constraints are enforced **mechanically via custom linters and structural tests**."

And the escalation rule, which is the sentence to remember:

> "When documentation falls short, we **promote the rule into code**."

---

## Why prose loses

`[INFERENCE]` Four independent reasons, and they compound:

1. **It can be ignored silently.** Nothing reports a violation, so you find out during review —
   or later.
2. **It costs context every session**, whether relevant or not. See
   [[Context Window as a Budget]].
3. **It dilutes the rules around it.** `[FACT]` "When everything is 'important,' nothing is."
   Each rule added makes the others weaker.
4. **It rots invisibly.** `[FACT]` A monolithic instruction file becomes "a graveyard of stale
   rules… agents can't tell what's still true."

A check has none of these properties. It fires or it does not, it costs nothing when the code
is clean, it does not compete with other checks, and a stale check fails loudly.

---

## The promotion ladder

Same rule, ascending strength. `[INFERENCE]` Take the **weakest rung that actually holds** —
stronger controls cost more and constrain more.

| Rung | Form | Strength |
|---|---|---|
| 1 | a line in the instruction file | advisory; always-on context cost |
| 2 | a skill, loaded on demand | advisory; no idle cost |
| 3 | **a remediation message on an existing check** | **enforced, and it teaches** |
| 4 | a new lint rule or test | enforced |
| 5 | a structural / architecture test | enforced, catches whole classes |
| 6 | a hook that blocks the action | absolute within its scope |
| 7 | a permission rule | absolute, blunt |

Rung 3 is the sweet spot and the most underused — you usually already have the check and are
simply not using its output as a teaching surface. See [[Feedback Quality]].

---

## Structural tests: the underused rung `[FACT]`

OpenAI's architecture is a fixed layer order — `Types → Config → Repo → Service → Runtime → UI`
— with cross-cutting concerns entering only through an explicit `Providers` interface.
"Anything else is disallowed and enforced mechanically."

Böckeler's parallel example: a pre-commit or agent hook running **ArchUnit** tests that check
module boundary violations.

`[INFERENCE]` A structural test converts "please respect the architecture" — a sentence no
model can reliably act on across a thousand edits — into a fact about the repository. It is
the single highest-leverage rung for a codebase agents will touch repeatedly.

---

## Why this is now affordable `[FACT]`

The historical objection was that a bespoke lint rule cost more to write than the bugs it
caught. That has inverted:

- OpenAI's custom linters were "Codex-generated, of course."
- Böckeler: agents "can help write structural tests, generate draft rules from observed
  patterns, scaffold custom linters, or create how-to guides from codebase archaeology."

`[INFERENCE]` And the payoff grew as well as the cost falling: a rule now teaches **every
future agent run**, not just future humans. Write the rule from three examples of the agent's
own mistakes and it takes minutes.

---

## The limit

`[INFERENCE]` Not everything is mechanisable. "Is this the right abstraction?" and "does this
match the design intent?" resist deterministic checks — that is what inferential controls are
for. The rule is not *"never write prose."* It is:

> **If a check could express it, a sentence should not be doing the job.**

Prose is for what checks cannot reach, and for pointing at where the checks live.

---

## The audit

Open your instruction file. For each rule ask:

1. Could a **command** detect a violation? → move it to rung 4 or 5.
2. Does a check already cover it, badly explained? → move it to rung 3, rewrite the message.
3. Is it a hard boundary rather than a preference? → rung 6 or 7.
4. None of the above? → keep it, and make it specific.

`[INFERENCE]` Most instruction files shrink substantially and get *more* effective, because
the surviving lines stop competing with rules that a program should have been enforcing.

---

## Related

- [[Feedback Quality]] · [[Guides and Sensors]] · [[Fix the Class Not the Instance]]
- [[Instruction File Design]] · [[Context Window as a Budget]] · [[Claude Code Hooks]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Harness Engineering for Coding Agent Users]]
