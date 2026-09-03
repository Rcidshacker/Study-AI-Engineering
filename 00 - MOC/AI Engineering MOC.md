---
title: AI Engineering MOC
aliases:
  - Start here
  - Home
tags:
  - moc
  - hub
status: evergreen
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# AI Engineering MOC

The central map. Everything else hangs off this.

> [!abstract] The one idea
> `Agent = Model + Harness`. You cannot change the model. Everything you *can* change is the
> harness, the loops that run on it, and the graphs that coordinate those loops — and each
> layer can only be as good as the one beneath it.

---

## Start here

| If you want… | Read |
|---|---|
| The conceptual frame, corrected | [[The Unified Mental Model]] |
| A plan | [[Learning Roadmap]] |
| To build something today | [[Coding Agent Harness]] |
| To know what is actually verified | [[Sources MOC]] |
| Definitions, with attribution | [[Glossary]] |

---

## The five layers

```text
GRAPH     how several agents and loops cooperate      →  [[Graph Engineering]]
LOOP      how one agent iterates to a goal            →  [[Loop Engineering]]
HARNESS   what it can do, see, remember, be checked by → [[Harness Engineering]]
CONTEXT   what it knows this turn                     →  [[Context Engineering]]
PROMPT    how this turn is phrased                    →  [[Prompt Engineering vs Context Engineering]]
```

Cumulative, not successive. Each layer keeps the ones below it.

---

## By area

### Foundations
[[Agent Engineering]] · [[Agent Loops]] · [[Agent State]] · [[Agent Orchestration]] ·
[[Context Engineering]] · [[Context Window as a Budget]] · [[External State]] ·
[[The Unified Mental Model]]

### The substrate
[[Harness Engineering]] · [[Harness Architecture]] · [[Harness Components]] ·
[[Guides and Sensors]] · [[Inner Harness vs Outer Harness]] · [[When Not to Build a Harness]] ·
[[Harness Beats Model Choice]] · [[Lineage of the Word Harness]]

### Verification — the part everything else depends on
[[The Verification Gap]] · [[False Completion]] · [[Generator Evaluator Separation]] ·
[[Feedback Quality]] · [[Feature List as Harness Primitive]] · [[Fix the Class Not the Instance]]

### The runtime
[[Loop Engineering]] · [[Loop Types]] · [[Inner Loops and Outer Loops]] ·
[[Stopping Conditions]] · [[Ralph Loop]]

### The system
[[Graph Engineering]] · [[Graph Engineering Origin and Fact-Check]]

### Claude Code
[[Claude Code]] · [[Claude Code Architecture]] · [[Claude Code as a Harness]] ·
[[Claude Code Loops]] · [[Claude Code Graphs]] · [[Claude Code Hooks]] ·
[[Claude Code Implementation Notes]] — full map at [[Claude Code MOC]]

### Applied
[[Coding Agent Harness]] · [[Autonomous Test Fixer]] · [[Production Coding Agent]] ·
[[Scenarios MOC]]

### Comparisons
[[Harness vs Loop vs Graph]] · [[Prompt Engineering vs Context Engineering]]

### Evidence
[[Sources MOC]] · [[Repository Index]] · [[Glossary]] ·
[[Research Integrity in Agent-Assisted Research]]

---

## The six claims this vault is built on

Each is traceable to a source note. Confidence is stated, not implied.

| Claim | Confidence | Where |
|---|---|---|
| The environment around the model explains more outcome variance than model choice, on long tasks | medium-high — no controlled study exists | [[Harness Beats Model Choice]] |
| Most agent unreliability reduces to the agent being unable to tell it was wrong | high | [[The Verification Gap]] |
| The thing that did the work must not decide the work is done | high | [[Generator Evaluator Separation]] |
| Anything the agent reads is a prompt, including error messages | high — two independent arrivals | [[Feedback Quality]] |
| Harness, loop and graph are layers, not alternatives | medium-high | [[The Unified Mental Model]] |
| Fix the class of failure, not the instance | high — three independent arrivals | [[Fix the Class Not the Instance]] |

---

## How to read this vault

- `[FACT]` I read it in a primary source · `[PRACTICE]` community practice ·
  `[OPINION]` attributed position · `[INFERENCE]` my synthesis ·
  `[UNVERIFIED]` could not confirm · `[CAUTION]` real but caveated.
- **Claims live in source notes; concept notes cite source notes.** One place to correct each
  fact.
- Every source note carries a `verified:` date. Treat anything months old as stale.
- The vault records its own [[Research Integrity in Agent-Assisted Research|worst failure]]
  rather than hiding it. That is deliberate.

---

## Related

- [[Agent Engineering MOC]] · [[Claude Code MOC]] · [[Harness Loop Graph MOC]] · [[Sources MOC]]
