---
title: Harness vs Loop vs Graph
aliases:
  - Comparing the three
tags:
  - comparison
  - harness-engineering
  - loop-engineering
  - graph-engineering
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Harness vs Loop vs Graph

> [!abstract] One line
> They are not alternatives. They are **layers**, and each one can only be as good as the one
> beneath it.

> [!note] Rewritten 2026-09-04
> The earlier version compared them as peers. The conceptual argument lives in
> [[The Unified Mental Model]]; this note is the practical side-by-side.

---

## Side by side

| | **Harness** | **Loop** | **Graph** |
|---|---|---|---|
| Shapes | the substrate | the runtime | the system |
| Answers | what can it do, see, remember, be checked by? | how does it iterate until done? | how do many of these cooperate? |
| Unit | a capability or a control | an iteration | a node |
| Artefacts | `CLAUDE.md`, tests, hooks, permissions, `init.sh`, progress files | trigger, goal, verifier, budget, stop condition | nodes, edges, shared state, routing rules |
| Named | early 2026, contested | June 2026, Osmani | July 2026, from a joke |
| Established? | emerging | emerging | barely |
| Fails as | ships broken code confidently | never stops, or stops too early | agents fight; nobody can tell what happened |
| Costs | maintenance, context budget | tokens, unattended risk | tokens × N, latency, opacity |

---

## The dependency, which is the whole point

```text
GRAPH     can only route work that a LOOP can execute
   ▲
LOOP      can only converge on a signal the HARNESS supplies
   ▲
HARNESS   can only check what the ENVIRONMENT exposes
```

`[INFERENCE]` Three consequences that people learn expensively:

1. **A loop cannot create a verification signal.** If no command fails when the change is
   wrong, iterating produces more unverified output, faster.
2. **A graph cannot either.** It relocates the check to a fresh context — valuable — but a
   reviewer with no tests to run is still guessing.
3. **Therefore feedback investment dominates.** `[FACT]` The course's ranking agrees: the
   feedback subsystem "usually has the lowest investment and highest return."

---

## What each layer actually buys

**Harness** — turns a text predictor into something that can act, and can be told it is wrong.
Without it: no tools, no memory across sessions, no way to detect failure. `[FACT]` And it is
not optional at scale: "even a frontier coding model… running in a loop across multiple
context windows will fall short" from a high-level prompt alone.

**Loop** — moves you from *inside* to *outside*. Without it, your attention is the throughput
ceiling. `[FACT]` "Loop engineering is replacing yourself as the person who prompts the agent."

**Graph** — buys **structural** independence, which no amount of in-loop checkpointing
provides. `[FACT]` "A graph doesn't give you more checkpoints; it moves the check — from inside
the agent to a standalone node with a fresh context." Also parallelism, and declared rollback
paths.

---

## The decision table

| Situation | Build |
|---|---|
| The agent gets facts wrong | **harness**: context, docs it can read |
| The agent ships broken code | **harness**: a real end-to-end check, and a hook that runs it |
| The agent repeats a mistake | **harness**: a rule, a linter, a teaching error message |
| You are typing "continue" repeatedly | **loop** |
| You want work done while you sleep | **loop** + isolation + budgets |
| You cannot trust a finding | **graph**: an independent verifier |
| Genuinely independent parallel work | **graph** + worktrees |
| It "feels complicated" | **none of these** — write down what done means first |

---

## The failure of skipping a layer `[INFERENCE]`

The characteristic expensive mistake is a five-agent pipeline — planner, researcher, coder,
tester, reviewer — on a project whose actual problem is that the test command was never
written down in `CLAUDE.md`. The graph multiplies token spend, latency and failure surface. It
does not add the missing signal.

The counter-example to keep in view: `[FACT]` [[GitHub - SWE-agent mini-swe-agent]] is a
**single agent, single loop, ~100 lines**, and scores >74% on SWE-bench Verified. Whatever you
add above the substrate should beat that on your task.

---

## Maturity, honestly

| Layer | Primary sources | Verified evidence |
|---|---|---|
| Harness | OpenAI (2026-02-11), Anthropic (2025-11-26), Thoughtworks (2026-04-02), LangChain (2026-03-10) | detailed first-party engineering reports; no controlled study |
| Loop | Osmani; two readable open-source implementations | working code; reported practitioner consensus |
| Graph | Anthropic's five patterns (2024-12-19) | the *patterns* are documented; the *discipline* has ~2 months and a debunked statistic |

See [[Graph Engineering Origin and Fact-Check]] for what is fabricated in the graph discourse.

---

## Related

- [[The Unified Mental Model]] · [[Harness Engineering]] · [[Loop Engineering]] · [[Graph Engineering]]
- [[Harness Components]] · [[Loop Types]] · [[Graph vs Workflow]] · [[When Not to Build a Harness]]
- [[Harness Loop Graph MOC]]
