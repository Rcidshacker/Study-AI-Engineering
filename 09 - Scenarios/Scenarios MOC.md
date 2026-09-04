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

## The scenarios

| Scenario | The failure it addresses | Note |
|---|---|---|
| **Building a feature safely** | modifying a live repository without collateral damage | [[Coding Agent Harness]] — the reference layout, built in four stages |
| **The agent edits the wrong files** | scope expressed as a sentence rather than a boundary | [[Sandboxing and Permissions]] — layered controls, weakest to absolute |
| **The agent writes code but doesn't verify it** | unit tests pass while the feature is broken end to end | [[The Verification Gap]] — the four moves, by return |
| **Autonomous test fixing** | inspect → diagnose → modify → test → analyse → retry → verify → **stop**, unattended | [[Autonomous Test Fixer]] |
| **Research agent building a knowledge base** | fluent, confident, fabricated output that nothing contradicts | [[Research Agent]] |
| **Skill routing at scale** | 20+ skills, and what actually breaks (it is not intent matching) | [[Skill Routing]] |
| **Multi-agent coding team** | planner, researcher, coder, tester, reviewer — and which roles survive scrutiny | [[Multi Agent Coding System]] |
| **Production coding agent** | agents writing most of a real codebase without it rotting | [[Production Coding Agent]] |

`[INFERENCE]` Read them in that order. Each assumes the controls built by the one before, and
the ordering matches the investment ranking in [[When Not to Build a Harness]] — feedback
first, orchestration last.

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

## Three scenarios that resolved to "build less"

`[INFERENCE]` Worth noting together, because the pattern repeats: the research kept ending
somewhere cheaper than the question assumed.

| Question | Expected answer | What the sources support |
|---|---|---|
| How do I route 20+ skills? | a router or a skill graph | **Nothing.** The router exists and is the model. The real constraint is a documented **description budget** at 1% of the context window — [[Skill Routing]] |
| How do five agents divide the work? | a five-role pipeline | **Two roles.** Coder plus a fresh-context reviewer; a researcher when context demands it. Tester and planner rarely justify a separate agent — [[Multi Agent Coding System]] |
| How do I make research reliable? | better prompts and more sources | **A URL liveness check.** One command, under a minute, catching what an hour of fluent writing produced — [[Research Agent]] |

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
