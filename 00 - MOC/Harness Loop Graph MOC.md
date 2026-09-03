---
title: Harness Loop Graph MOC
aliases:
  - The three disciplines
  - Harness Engineering MOC
  - Loop Engineering MOC
  - Graph Engineering MOC
tags:
  - moc
status: evergreen
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Harness · Loop · Graph MOC

The three named disciplines, their relationship, and what is actually established about each.

> [!warning] The relationship, up front
> They are **not siblings**. They are layers, and each can only be as good as the one beneath
> it. The full argument, including the correction to the common sibling diagram, is in
> [[The Unified Mental Model]].

```text
GRAPH     the system    nodes, edges, shared state, routing
LOOP      the runtime   trigger, act, verify, persist, stop
HARNESS   the substrate instructions · tools · environment · state · feedback
```

---

## Maturity, honestly

| | Named | Attribution | Established? |
|---|---|---|---|
| **Harness engineering** | early 2026 | **contested** — Hashimoto, OpenAI, Trivedy all cited | emerging; concept much older |
| **Loop engineering** | June 2026 | Addy Osmani | emerging; three practitioners in one week |
| **Graph engineering** | July 2026 | began as a **joke** | barely; practice predates it by 18 months |

Details: [[Lineage of the Word Harness]] · [[Source - Addy Osmani Loop Engineering]] ·
[[Graph Engineering Origin and Fact-Check]]

---

## Layer 1 — Harness

**What it is:** everything around the model. `Agent = Model + Harness`.

**Core:** [[Harness Engineering]] · [[Harness Architecture]] · [[Harness Components]]

**The framework to use:** [[Guides and Sensors]] — every control is a guide or a sensor, and
computational or inferential. Build in all four quadrants.

**Scope and judgement:** [[Inner Harness vs Outer Harness]] · [[When Not to Build a Harness]] ·
[[Harness Beats Model Choice]]

**Verification, which is the load-bearing subsystem:**
[[The Verification Gap]] · [[False Completion]] · [[Feedback Quality]] ·
[[Feature List as Harness Primitive]] · [[Generator Evaluator Separation]]

**The practice:** [[Fix the Class Not the Instance]]

**State:** [[Agent State]] · [[External State]] · [[Context Window as a Budget]]

**Primary sources:** [[Source - OpenAI Harness Engineering]] ·
[[Source - Anthropic Effective Harnesses for Long-Running Agents]] ·
[[Source - Harness Engineering for Coding Agent Users]] ·
[[Source - Anatomy of an Agent Harness]] · [[Source - Wikipedia Agent Harness]]

---

## Layer 2 — Loop

**What it is:** replacing yourself as the person who prompts the agent.

**Core:** [[Loop Engineering]] · [[Agent Loops]] · [[Loop Types]] ·
[[Inner Loops and Outer Loops]]

**Control:** [[Stopping Conditions]] — five terminal states, not one.

**Worked patterns:** [[Ralph Loop]] · [[Autonomous Test Fixer]]

**Implementations read in full:** [[GitHub - snarktank ralph]] ·
[[GitHub - SWE-agent mini-swe-agent]]

**In Claude Code:** [[Claude Code Loops]]

**Primary source:** [[Source - Addy Osmani Loop Engineering]]

---

## Layer 3 — Graph

**What it is:** what a loop becomes when work needs specialisation, parallelism, shared state
and independent verification.

**Core:** [[Graph Engineering]] · [[Agent Orchestration]]

**Read this first:** [[Graph Engineering Origin and Fact-Check]] — the term's origin and the
fabricated statistics circulating with it.

**In Claude Code:** [[Claude Code Graphs]]

**Primary source:** [[Source - Anthropic Building Effective Agents]] — the five patterns,
published eighteen months before the term existed.

---

## Choosing between them

| Situation | Layer |
|---|---|
| It gets facts wrong | harness — context |
| It ships broken code | harness — feedback |
| It repeats a mistake | harness — a control, not a sentence |
| You keep typing "continue" | loop |
| You want work done overnight | loop + isolation + budgets |
| You cannot trust a finding | graph — an independent verifier |
| Genuinely independent parallel work | graph + worktrees |
| It "feels complicated" | none — define done first |

Full comparison: [[Harness vs Loop vs Graph]].

---

## The one thing to remember `[INFERENCE]`

**Neither a loop nor a graph can create a verification signal the harness does not supply.**
Adding orchestration on top of weak feedback multiplies unverified output. Fix the substrate
first — and note that [[GitHub - SWE-agent mini-swe-agent]] reaches >74% on SWE-bench Verified
with a single agent, a single loop, and about a hundred lines.

---

## Related

- [[AI Engineering MOC]] · [[Agent Engineering MOC]] · [[Claude Code MOC]] · [[Sources MOC]]
- [[Learning Roadmap]] · [[Glossary]]
