---
title: Glossary
aliases:
  - Terminology
tags:
  - reference
  - glossary
status: evergreen
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Glossary

> [!note] Rewritten 2026-09-04
> The earlier version defined terms without attribution. Every entry below is either a quoted
> definition from a source I read, or marked as my own synthesis. Terms whose meaning is
> **contested** say so.

---

## The core equation

**Agent = Model + Harness** `[FACT]`
The field's organising formula. "If you're not the model, you're the harness."
— [[Source - Anatomy of an Agent Harness]]

---

## A–C

**Agent** `[FACT]` — "systems where LLMs dynamically direct their own processes and tool usage,
maintaining control over how they accomplish tasks." Contrast **workflow**.
— [[Source - Anthropic Building Effective Agents]]

**Agent-computer interface (ACI)** `[FACT]` — the tool definitions, documentation and output an
agent reads. Anthropic names crafting it as one of three core principles.

**Agent harness / agent scaffolding** `[FACT]` — "the software infrastructure surrounding a
large language model that enables it to operate as an AI agent. It manages tool use, memory,
state persistence, execution environments and feedback loops, as opposed to the model's
internal reasoning." — [[Source - Wikipedia Agent Harness]]

**Anchor** `[FACT]` — what pins a loop to reality: real outcomes, ground-truth data, human
spot-checks. "The part everyone skips." See [[Graph Engineering]].

**Clean state** `[FACT]` — "the kind of code that would be appropriate for merging to a main
branch: no major bugs, orderly and well-documented," such that the next session can start work
without first clearing a mess. — [[Source - Anthropic Effective Harnesses for Long-Running Agents]]

**Compaction** `[FACT]` — summarising a filling context window. "Compaction isn't sufficient"
as a memory strategy; it "doesn't always pass perfectly clear instructions to the next agent."

**Computational control** `[FACT]` — deterministic, CPU, milliseconds to seconds, reliable.
Tests, linters, type checkers, structural analysis. Contrast **inferential**.
— [[Source - Harness Engineering for Coding Agent Users]]

**Context engineering** `[FACT]` — governing "what information the model sees at a given
moment." Its containment relation to the harness is **contested** — see
[[The Unified Mental Model]].

---

## D–H

**Evaluator-optimizer** `[FACT]` — "one LLM call generates a response while another provides
evaluation and feedback in a loop." One of Anthropic's five patterns; the ancestor of
[[Generator Evaluator Separation]].

**False completion** `[INFERENCE — my name for a documented failure]` — the agent declares done
when it is not. Documented as: a later agent "would look around, see that progress had been
made, and declare the job done."

**Feature list** `[FACT]` — a comprehensive JSON file of end-to-end behaviours, all starting
`"passes": false`, which agents may only change the status of. See
[[Feature List as Harness Primitive]].

**Feedforward / feedback controls** → see **guide** / **sensor**.

**Goal loop** `[FACT]` — a loop that runs until an independent evaluator confirms a condition
is met **or judges it impossible**. Contrast **interval loop**, which repeats on a clock with
no cumulative progress.

**Graph engineering** `[CAUTION]` — coordinating multiple agents, loops and evaluators as
nodes, edges, shared state and routing. **Named around July 2026, from a joke.** The practice
predates the term by at least eighteen months. See [[Graph Engineering Origin and Fact-Check]].

**Guide** `[FACT]` — a feedforward control: anticipates behaviour and steers **before** the
agent acts. — [[Source - Harness Engineering for Coding Agent Users]]

**Harness engineering** `[CAUTION]` — building everything around the model so it behaves
reliably. Emerged **early 2026**; attribution **contested**. See [[Lineage of the Word Harness]].

**Harnessability** `[FACT]` — a property of the *codebase*: how amenable it is to being
regulated by controls at all. No tests, no types, no boundaries ⇒ little to attach to.

---

## I–O

**Inferential control** `[FACT]` — semantic, GPU/NPU, slower, non-deterministic. AI code
review, LLM-as-judge.

**Inner harness / outer harness** `[FACT]` — what the vendor ships versus what you assemble on
top. See [[Inner Harness vs Outer Harness]].

**Inner loop / outer loop** `[FACT]` — what the agent does within a turn versus what you build
around whole sessions. See [[Inner Loops and Outer Loops]].

**Initializer agent** `[FACT]` — a first session with a *different prompt*, whose job is to set
up `init.sh`, a progress file, a feature list, and a baseline commit.

**Loop engineering** `[FACT]` — "replacing yourself as the person who prompts the agent. You
design the system that does it instead." — Addy Osmani, mid-2026.

**Orchestrator-workers** `[FACT]` — "a central LLM dynamically breaks down tasks, delegates
them to worker LLMs, and synthesizes their results." Use when you cannot predict the subtasks.

---

## P–Z

**Prompt chaining** `[FACT]` — a sequence of LLM calls with programmatic **gates** between
steps.

**Ralph / Ralph Wiggum loop** `[FACT]` — repeated fresh agent runs against the same prompt
until a completion sentinel appears, with memory in git and files. Named after Geoffrey
Huntley's pattern; cited by name in OpenAI's engineering report. See [[Ralph Loop]].

**ReAct** `[FACT]` — the peer-reviewed pattern of a model alternating reasoning and acting in a
loop. Prior art for every agent loop in use.

**Sensor** `[FACT]` — a feedback control: observes **after** the agent acts and lets it
self-correct. Most powerful when its output is "optimised for LLM consumption."

**Steering loop** `[FACT]` — the human's job: iterating on the controls whenever an issue
happens **multiple times**. See [[Fix the Class Not the Instance]].

**System of record** `[FACT]` — the repository. "Anything it can't access in-context while
running effectively doesn't exist."

**Verification gap** `[INFERENCE — my name]` — the agent can act but cannot observe the
consequence at the level a user would. The root of most agent unreliability. See
[[The Verification Gap]].

**Workflow** `[FACT]` — "systems where LLMs and tools are orchestrated through predefined code
paths." The dividing line from an agent is **who decides the path**.

**Worktree isolation** `[FACT]` — each parallel agent in its own git worktree, so file
collisions are physically impossible. The prerequisite for parallelism.

---

## Terms to use carefully

| Term | Why |
|---|---|
| "harness" unqualified | at least three unrelated meanings — test harness, evaluation harness, and a CI/CD product. Say *agent harness* |
| "graph engineering" | ~2 months old, born from a parody, and carrying at least one fabricated statistic |
| "loop engineering" | real and useful, but roughly a year old — not an established body of knowledge |
| "context engineering" | its relation to the harness is genuinely disputed |

---

## Related

- [[Lineage of the Word Harness]] · [[The Unified Mental Model]] · [[Sources MOC]]
- [[Graph Engineering Origin and Fact-Check]]
