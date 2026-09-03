---
title: Agent Engineering MOC
aliases:
  - Agent engineering map
tags:
  - moc
  - agent-engineering
status: evergreen
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Agent Engineering MOC

The activity, rather than the layers. [[Harness Loop Graph MOC]] organises by *layer*; this
one organises by **what you are trying to make happen**.

> [!abstract] The job, in seven words `[FACT]`
> "Design environments, specify intent, and build feedback loops."
> — [[Source - OpenAI Harness Engineering]]

---

## The concepts

[[Agent Engineering]] — what an agent is, and what the job became
[[Agent Loops]] — a model in a `while` loop with tools
[[Agent State]] — the model has none; you supply all of it
[[Agent Orchestration]] — coordinating several agents, and when not to
[[Context Engineering]] — what is in scope when it decides
[[The Unified Mental Model]] — how it all fits, corrected

---

## Organised by what you want

### "Make it do the right thing"
[[Instruction File Design|Instructions]] as a map not a manual · [[Context Window as a Budget]] ·
[[Guides and Sensors]] — feedforward controls

### "Make it able to act"
[[Harness Components]] — tools and environment ·
[[Sandboxing and Permissions]] · [[Worktree Isolation]]

### "Make it know when it's wrong"
[[The Verification Gap]] — the root of almost everything ·
[[Feedback Quality]] · [[Generator Evaluator Separation]] · [[False Completion]]

### "Make it remember"
[[External State]] · [[Agent State]] · [[Feature List as Harness Primitive]]

### "Make it run without me"
[[Loop Engineering]] · [[Loop Types]] · [[Stopping Conditions]] · [[Ralph Loop]] ·
[[Autonomous Test Fixer]]

### "Make several of them cooperate"
[[Agent Orchestration]] · [[Graph Engineering]] · [[Claude Code Graphs]]

### "Make it stay good"
[[Fix the Class Not the Instance]] · [[Harness Debt and Garbage Collection]] ·
[[Production Coding Agent]]

---

## The failure taxonomy

Every documented agent failure I found reduces to one of these five. Diagnose before building.

| Failure | Root cause | Fix |
|---|---|---|
| Wrong conventions, wrong tools | it was never told | [[Instruction File Design]] |
| Cannot proceed, asks you to run things | capability missing | [[Harness Components]] |
| Ships broken code confidently | **cannot observe consequences** | [[The Verification Gap]] |
| Declares victory early | no durable definition of done; grades itself | [[False Completion]] |
| Re-solves solved problems | state lives in the transcript | [[External State]] |

`[INFERENCE]` Row three is the majority case, and the one most often misdiagnosed as "the model
isn't good enough."

---

## The three principles worth memorising `[FACT]`

From [[Source - Anthropic Building Effective Agents]]:

1. Maintain **simplicity** in your agent's design.
2. Prioritise **transparency** by explicitly showing planning steps.
3. Carefully craft your **agent-computer interface** through thorough tool documentation and
   testing.

`[INFERENCE]` Principle 3 is the underrated one: the ACI is every piece of text the agent
reads — tool descriptions, error messages, file names, log output, test names. Designing that
text *is* the job. See [[Feedback Quality]].

---

## What is genuinely new about this `[INFERENCE]`

Loops, graphs, feedback control and state machines are decades old. Three things changed:

1. The unit of work is **non-deterministic**, so the design goal is graceful recovery rather
   than correctness by construction.
2. The unit of work is **cheap and parallel**, so throughput can exceed human review capacity —
   which changes merge philosophy and the value of mechanical enforcement.
3. The unit of work **reads its own environment**, so the environment became a programming
   surface. Writing a linter error message is now prompt engineering.

The third is the genuinely novel one. See [[Executable Rules Beat Written Rules]].

---

## Related

- [[AI Engineering MOC]] · [[Harness Loop Graph MOC]] · [[Claude Code MOC]] · [[Sources MOC]]
- [[Learning Roadmap]] · [[Glossary]]
