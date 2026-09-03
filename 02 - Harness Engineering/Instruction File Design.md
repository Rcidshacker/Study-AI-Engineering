---
title: Instruction File Design
aliases:
  - CLAUDE.md design
  - AGENTS.md design
  - Map not manual
tags:
  - harness-engineering
  - context-engineering
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Instruction File Design

> [!abstract] One line
> A table of contents, not an encyclopedia. Around 100–200 lines, pointing at a structured
> `docs/` tree that is read on demand.

---

## The four documented failure modes of one big file `[FACT]`

OpenAI tried the monolithic `AGENTS.md` and it failed in named, predictable ways:

1. **Context is a scarce resource.** "A giant instruction file crowds out the task, the code,
   and the relevant docs — so the agent either misses key constraints or starts optimizing for
   the wrong ones."
2. **Too much guidance becomes non-guidance.** "When everything is 'important,' nothing is.
   Agents end up pattern-matching locally instead of navigating intentionally."
3. **It rots instantly.** "A monolithic manual turns into a graveyard of stale rules. Agents
   can't tell what's still true, humans stop maintaining it, and the file quietly becomes an
   attractive nuisance."
4. **It's hard to verify.** "A single blob doesn't lend itself to mechanical checks (coverage,
   freshness, ownership, cross-links), so drift is inevitable."

`[INFERENCE]` Failure 2 is the one that surprises people: **adding a correct rule can make the
file worse.** Instructions are not additive; they compete.

---

## The fix `[FACT]`

> "So instead of treating `AGENTS.md` as the encyclopedia, we treat it as the **table of
> contents**."

A short file (~100 lines) injected into context as a **map**, with the knowledge base living in
a structured `docs/` directory as the system of record:

```text
AGENTS.md              ~100 lines: a map
ARCHITECTURE.md
docs/
├── design-docs/
│   ├── index.md
│   └── core-beliefs.md
├── exec-plans/
│   ├── active/
│   ├── completed/
│   └── tech-debt-tracker.md
├── generated/
│   └── db-schema.md
└── product-specs/
```

`[FACT]` Claude Code's own guidance is compatible and slightly more generous: target **under
200 lines** — "longer files still load in full but may reduce adherence" — and "if something
only matters for specific tasks, move it to a **skill** or a **path-scoped rule** so it loads
only when needed."

---

## What belongs in the file

`[INFERENCE]` Four sections, in this order:

```markdown
## Commands          the verification command first — highest-value line in the file
## Stack             versions, and anything a wrong guess would break
## Hard constraints  short, absolute, non-negotiable
## Where things are  pointers to docs/, with one line each on when to read them
```

**What does not belong:**

| Keep out | Because | Put it |
|---|---|---|
| Anything a check could enforce | prose is advisory | a lint rule — [[Executable Rules Beat Written Rules]] |
| Task-specific procedure | costs context every session | a skill |
| Domain conventions (tests, API, frontend) | only relevant sometimes | a path-scoped rule |
| Explanations of *why* | the agent does not need the essay to comply | `docs/`, linked |
| Anything you are not sure is still true | stale rules cost the same as live ones and mislead | delete it |

---

## The single highest-value line

```text
Full verification: make check
```

`[INFERENCE]` One command, in the file the agent always reads. Everything downstream — hooks,
`/goal`, external loops, CI — can invoke it. `[FACT]` The feedback subsystem "usually has the
lowest investment and highest return," and this line is the cheapest part of it.

---

## Maintenance

`[FACT]` OpenAI's answer to rot was not discipline but **mechanism**: golden principles encoded
in the repo, plus background tasks on a cadence that scan for deviations and open targeted
refactoring PRs. See [[Harness Debt and Garbage Collection]].

`[INFERENCE]` The minimum viable version: when you add a rule, ask what would remove it. A rule
with no removal condition is a permanent context cost. Re-audit after every model upgrade —
`[FACT]` "as models get stronger, some components stop being critical."

---

## The audit

For each line: does it apply to **most** sessions? Could a **check** enforce it instead? Is it
**still true**? Three questions, and most files halve.

---

## Related

- [[Context Window as a Budget]] · [[Executable Rules Beat Written Rules]] · [[Harness Components]]
- [[The Repository as System of Record]] · [[Claude Code as a Harness]] · [[Coding Agent Harness]]
- [[Source - OpenAI Harness Engineering]]
