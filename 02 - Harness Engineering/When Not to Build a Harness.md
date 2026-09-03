---
title: When Not to Build a Harness
aliases:
  - Harness over-engineering
  - Do you need a harness
tags:
  - harness-engineering
  - judgement
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# When Not to Build a Harness

> [!abstract] One line
> Harness investment pays off in proportion to **repetition × autonomy × stakes**. When all
> three are low, the harness is a hobby.

---

## The scope limit, stated by the sources themselves `[FACT]`

[[Source - Wikipedia Agent Harness]]:

> "A minimal harness is unnecessary for a single prompt-and-response exchange, but becomes
> important as tasks grow multi-step, tool-oriented, or long-running."

[[Source - OpenAI Harness Engineering]], on its own results:

> "This behavior depends heavily on the specific structure and tooling of this repository and
> should not be assumed to generalize without similar investment—at least, not yet."

`[INFERENCE]` The second quote is the one to remember. OpenAI is not describing a recipe; they
are describing what *their* several-month investment bought *them*. Copying the artefacts
without the investment gets you the artefacts.

---

## The three variables

| Variable | Question | Low → |
|---|---|---|
| **Repetition** | Will this happen again, many times? | a control amortises over zero uses |
| **Autonomy** | Will the agent run without you watching? | you *are* the sensor; you don't need another |
| **Stakes** | What does a wrong answer cost? | correcting in chat is cheaper than any control |

`[INFERENCE]` They multiply rather than add. High stakes with zero repetition is a case for
**careful review**, not for a harness. High repetition with trivial stakes is a case for a
**snippet**, not a harness. You need at least two of the three to be meaningfully high.

---

## Cases where you should not build

- **One-off scripts and throwaway analysis.** You will read the output. That is the verification.
- **Exploration.** You do not know what "done" means yet, so you cannot write a stopping
  condition or a feature list. Explore first; harness the thing you decide to build.
- **Projects with no test surface at all.** `[INFERENCE]` A harness needs something to attach
  to. In a codebase with no types, no tests and no module boundaries, computational sensors
  have nowhere to grip. Build the test surface *first* — that work is valuable regardless of
  agents. See [[Harnessability]].
- **Before you have observed real failures.** `[FACT]` Claude Code's own documentation
  advises starting with `CLAUDE.md` and adding extensions "as specific triggers come up."
  Speculative controls are the instruction-file bloat that
  [[Source - OpenAI Harness Engineering]] documents as actively harmful.
- **When you are still choosing tools.** Outer-harness work transfers between agents; time
  spent learning one inner harness's quirks does not. See [[Inner Harness vs Outer Harness]].

---

## The over-engineering signatures `[INFERENCE]`

Watch for these in your own setup:

| Signature | What it usually means |
|---|---|
| A multi-agent graph before a test suite | [[The Verification Gap]] misdiagnosed as an orchestration problem |
| A 600-line `CLAUDE.md` | rules accumulated one incident at a time, never promoted or pruned — see [[Instruction File Design]] |
| Rules nothing enforces | comfort, not control — see [[Executable Rules Beat Written Rules]] |
| A component you cannot name a failure for | added from a checklist, not from experience |
| Loops with no budget | not automation; an unattended way to generate review work |
| A memory system before a progress file | solving the general case before the specific one |

The counter-example worth keeping in mind: `[FACT]`
[[GitHub - SWE-agent mini-swe-agent]] is roughly 100 lines, single-agent, single-loop, with no
memory system and no orchestration, and scores >74% on SWE-bench Verified. **Whatever you are
adding, it should beat that baseline for your task.**

---

## The minimum that is almost always worth it

Even for small projects, `[INFERENCE]` these four are cheap enough that the calculation rarely
goes the other way:

1. A short `CLAUDE.md` — stack, commands, hard constraints. Twenty lines.
2. **The verification commands written down in it.** The single highest-return item in the
   whole discipline.
3. A `.gitignore` and a clean tree, so the agent can use git as its undo.
4. Committing before you start anything unattended.

That is maybe fifteen minutes. Everything past it should be paid for by an observed failure.

---

## The order of investment

When you *have* decided to invest, spend in this order — it is roughly descending return:

```text
1. Feedback     verification commands, then make their output teach   ← start here
2. Instructions a map, not a manual
3. State        a progress file and a status artefact
4. Environment  reproducible setup, isolation for unattended runs
5. Tools        only what a real task actually needed
6. Loops        once done is machine-checkable
7. Graphs       once you have independent parallel work, or must not trust a finding
```

`[FACT]` The ranking of feedback first is [[Source - Learn Harness Engineering Course]]'s:
the feedback subsystem "usually has the lowest investment and highest return."

---

## Related

- [[Harness Engineering]] · [[Harness Components]] · [[Harnessability]]
- [[The Verification Gap]] · [[Instruction File Design]] · [[Claude Code as a Harness]]
- [[Source - Wikipedia Agent Harness]] · [[Source - OpenAI Harness Engineering]]
