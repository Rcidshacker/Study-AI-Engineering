---
title: Scenarios MOC
aliases:
  - Scenario index
tags:
  - moc
  - scenario
status: evergreen
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Scenarios MOC

Worked situations. Each one takes a failure people actually hit and designs the controls that
remove it, rather than describing a capability.

> [!note] Rewritten 2026-09-04
> The earlier version linked to scenario notes that did not exist. This version links only to
> what is written, and states plainly what is not.

---

## Written

| Scenario | The failure it addresses | Note |
|---|---|---|
| **Autonomous test fixing** | an agent must inspect → diagnose → modify → test → analyse → retry → verify → **stop**, unattended | [[Autonomous Test Fixer]] |
| **Production coding agent** | agents writing most of a real codebase without it rotting | [[Production Coding Agent]] |
| **Building a feature safely** | modifying a live repository without collateral damage | [[Coding Agent Harness]] — the reference layout, built in four stages |

---

## The failure modes these scenarios exist to remove

Each has a note; the scenarios are where they get composed into a working setup.

| Failure | Why it happens | Note |
|---|---|---|
| **Agent says done, isn't** | nothing in the environment defines "all of done", and the author grades itself | [[False Completion]] |
| **Agent writes but doesn't verify** | unit tests pass while the feature is broken end to end | [[The Verification Gap]] |
| **Agent edits the wrong files** | scope exists as a sentence, not as a rule | [[Wrong File Modification]] |
| **Loop never ends, or ends early** | one terminal state instead of five | [[Stopping Conditions]] |
| **Progress lost between sessions** | state lives in the transcript instead of on disk | [[External State]] |
| **Codebase degrades over time** | agents replicate existing patterns, including bad ones | [[Harness Debt and Garbage Collection]] |

---

## Not yet written

Listed honestly rather than linked, so the graph has no dead ends. Each is a real scenario from
the original research brief that this vault has the material for but has not yet composed:

- **Skill routing at scale** — 20+ skills, and the question of whether intent → skill → tools
  needs a graph, a routing layer, or nothing. `[INFERENCE]` The likely answer is *nothing*:
  skills already load on relevance, and a hand-built router is a layer competing with the model
  at something the model does well. The exception is when routing must be **deterministic** for
  compliance or cost reasons.
- **Multi-agent coding team** — planner, researcher, coder, tester, reviewer: how they
  communicate and who controls the loop. Design constraints are in [[Agent Orchestration]] and
  [[Claude Code Graphs]]; the four hard questions are division of labour, parallelism,
  rollback, and handoff.
- **Research agent producing a knowledge base** — the harness and loop for exactly the task
  that produced this vault. The failure it must prevent has already been demonstrated here:
  [[Research Integrity in Agent-Assisted Research]].

---

## How to use a scenario

`[INFERENCE]` Read the failure first, then the controls. The value is not the configuration —
it is the chain from *observed failure* → *class of failure* → *cheapest control that removes
the class*. Copying a configuration without that chain gives you someone else's controls for
problems you may not have, at a context cost you definitely will. See
[[When Not to Build a Harness]].

---

## Related

- [[Harness Loop Graph MOC]] · [[Coding Agent Harness]] · [[Learning Roadmap]]
- [[Harness Failure Modes]] · [[Loop Failure Modes]]
