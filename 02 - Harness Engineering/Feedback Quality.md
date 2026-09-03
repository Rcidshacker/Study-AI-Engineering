---
title: Feedback Quality
aliases:
  - Agent-readable errors
  - Positive prompt injection
tags:
  - harness-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Feedback Quality

> [!abstract] One line
> An error message is a prompt. It is delivered at the exact moment the agent is wrong, into
> the exact context where the fix belongs — and almost nobody writes it that way.

---

## The finding `[FACT]`

Two organisations, independently, two months apart:

**Thoughtworks** ([[Source - Harness Engineering for Coding Agent Users]], 2 April 2026) —
sensors are "particularly powerful when they produce signals that are optimised for LLM
consumption, e.g. custom linter messages that include instructions for the self-correction —
**a positive kind of prompt injection.**"

**OpenAI** ([[Source - OpenAI Harness Engineering]], 11 February 2026) — "Because the lints
are custom, **we write the error messages to inject remediation instructions into agent
context.**"

`[INFERENCE]` Independent convergence on a non-obvious technique is the strongest signal
available in this literature. Treat this as one of the few near-certain practices in the
field.

---

## Why the timing makes it powerful

`[INFERENCE]` A rule in `CLAUDE.md` is read at session start, competes with everything else
for context, and applies to a situation that has not arisen yet. An error message arrives:

- **exactly when** the rule is relevant,
- **exactly where** the violation is, with file and line,
- in a context the agent is already attending to,
- and **only when needed**, costing nothing otherwise.

It is a **guide delivered through a sensor** — the one place in [[Guides and Sensors]] where
the two directions combine, and it inherits the strengths of both.

---

## Anatomy of a good agent-facing error

```text
✗ error: no-any at src/api.ts:42

✓ error: no-any at src/api.ts:42
    This project parses external data at the boundary rather than trusting it.
    Define a zod schema in src/schemas/ and parse there.
    Pattern: docs/design-docs/core-beliefs.md#boundary-parsing
    Example: src/schemas/user.ts
```

Four parts, in order of value:

| Part | Why |
|---|---|
| **What to do instead** | the actual fix, not just the prohibition |
| **Why** | lets the agent generalise to cases the rule does not literally cover |
| **Where the pattern is documented** | a pointer the agent can follow on demand |
| **A concrete example in this repo** | the strongest signal of all — models pattern-match |

`[INFERENCE]` Note that this is also a better message for humans. Agent-legibility and
human-legibility point the same way here, which makes it easy to justify.

---

## Where to apply it

| Surface | Effort | Note |
|---|---|---|
| **Custom lint rules** | low | you control the message entirely |
| **Test assertion messages** | trivial | `assert x == y, "..."` — the most-neglected free win |
| **Type errors** | none | you cannot edit the compiler's text; compensate elsewhere |
| **CI failure output** | low | make the summary line say what to do |
| **Hook rejections** | low | a hook that blocks an action should say what action *is* allowed |
| **Custom scripts** | low | `check.sh` should print remediation, not just exit 1 |

`[INFERENCE]` Test failure messages are the highest-return item and the easiest to ignore. An
assertion that says `AssertionError: False is not true` teaches nothing. The same assertion
with a sentence of context turns every red test into a targeted instruction.

---

## The general principle

> **Anything the agent reads is a prompt. Write it as one.**

That includes error messages, file and directory names, test names, commit messages, log
lines, and the failure summaries your CI prints. `[INFERENCE]` This is the genuinely new
thing about harness engineering as a discipline: the environment is now a programming surface
aimed at a reader that reasons. Prose quality inside your tooling has become an engineering
property.

---

## The check

Pick your three most common agent mistakes. For each, ask: **when the agent makes this
mistake, what text does it see?** If that text does not tell it what to do instead, you have
found your next hour of work — and it will pay back more than another line in `CLAUDE.md`.

---

## Related

- [[Guides and Sensors]] · [[Fix the Class Not the Instance]] · [[Executable Rules Beat Written Rules]]
- [[The Verification Gap]] · [[Harness Components]]
- [[Source - Harness Engineering for Coding Agent Users]] · [[Source - OpenAI Harness Engineering]]
