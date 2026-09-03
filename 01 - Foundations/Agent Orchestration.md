---
title: Agent Orchestration
aliases:
  - Multi-agent orchestration
  - Graph Orchestration
tags:
  - agent-orchestration
  - graph-engineering
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Agent Orchestration

> [!abstract] One line
> Coordinating several agents. Genuinely useful in a narrow set of cases, and reached for far
> more often than those cases occur.

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose with an orchestra metaphor. This version is
> grounded in Anthropic's *Building Effective Agents* patterns, the official Claude Code
> feature set, and the failure analysis in the harness-engineering course.

---

## The vocabulary that already existed `[FACT]`

[[Source - Anthropic Building Effective Agents]] (December 2024) named five composable
patterns, eighteen months before "graph engineering":

| Pattern | Shape | Use when |
|---|---|---|
| **Prompt chaining** | linear, with programmatic gates between steps | the task decomposes into fixed subtasks |
| **Routing** | classify, then dispatch to a specialist | distinct categories, reliable classification |
| **Parallelization** | fan-out then aggregate — *sectioning* or *voting* | independent subtasks, or confidence through repetition |
| **Orchestrator-workers** | central LLM decomposes, delegates, synthesises | you **cannot predict** the subtasks |
| **Evaluator-optimizer** | generate ⇄ critique in a loop | clear criteria, and refinement measurably helps |

Use these as your vocabulary. `[INFERENCE]` Most "multi-agent architectures" being described
in 2026 are one of these five with new names.

---

## The distinction that decides your design `[FACT]`

> - "**Workflows** are systems where LLMs and tools are orchestrated through predefined code paths."
> - "**Agents** are systems where LLMs dynamically direct their own processes and tool usage."

**Who decides the path** is the question. Predefined path ⇒ workflow, and you get
predictability. Model-decided path ⇒ agent, and you get flexibility plus non-determinism at
every node. See [[Graph vs Workflow]].

---

## When orchestration earns its cost

`[INFERENCE]` Three cases, and they are narrower than the enthusiasm suggests:

**1. Verification.** A checker with a fresh context that did not write the code. This is the
only case with a *structural* justification rather than a convenience one: as
[[Source - Learn Harness Engineering Course]] puts it, a single loop's checkpoints fail
because "the judge and the judged share one brain." See [[Generator Evaluator Separation]].

**2. Context isolation.** `[FACT]` Subagents "run their own loops in isolated context,
returning summaries." Worth it when the input is small, the intermediate work is large, and
the output is small — research, audit, search. See [[Context Window as a Budget]].

**3. Genuine independence.** Work that does not share state and can proceed in parallel. This
requires real isolation first. See [[Worktree Isolation]].

---

## When it does not

| Symptom | Actual fix |
|---|---|
| The agent ships broken code | a test and a hook that runs it |
| The agent repeats a mistake | a rule, a linter, a better error message |
| The agent's context fills up | move detail into skills and files |
| One long task, one line of work | a loop, not a graph |

`[FACT]` Anthropic's own advice, stated three times in the post: "finding the simplest solution
possible… This might mean **not building agentic systems at all**," and "add multi-step
agentic systems only when simpler solutions fall short."

`[FACT]` The counter-example: [[GitHub - SWE-agent mini-swe-agent]] is single-agent,
single-loop, ~100 lines, and scores >74% on SWE-bench Verified. Whatever you orchestrate
should beat that on your task.

---

## The four hard problems `[FACT — the questions, from Lecture 14]`

When these arise, a loop has genuinely become a graph:

1. **Division of labour** — who goes first?
2. **Parallelism** — what can run at once?
3. **Rollback** — on failure, return to which node?
4. **Handoff** — how do agents share requirements, notes and results; and if the reviewer
   disagrees with the implementer, who wins?

`[INFERENCE]` Question 4 is where multi-agent systems actually fail. **The answer must be the
filesystem.** Messages between agents are lossy, unreviewable and gone after the run; a shared
findings file survives a crash, can be read by a human, and can be committed. Conflict
resolution must be an explicit rule, not an emergent property of who spoke last.

---

## What Claude Code provides `[FACT — official docs, 2026-09-04]`

| Primitive | Documented as |
|---|---|
| Subagents | "run their own loops in isolated context, returning summaries" |
| Dynamic workflows | "run many subagents from a script Claude writes, returning one result" |
| Agent teams | orchestrate teams of Claude Code sessions |
| Cross-session messaging | pass a message from one of your sessions to another |
| Agent view | manage multiple agents |
| Worktrees | run parallel sessions in isolation |

Implementation guidance in [[Claude Code Graphs]].

---

## The cost, stated honestly `[FACT]`

Anthropic: agent autonomy means "higher costs, and the potential for **compounding errors**."
`[INFERENCE]` Orchestration multiplies token spend, latency, failure surface, and — worst —
the difficulty of understanding what happened. Five parallel agents are easy to start and hard
to read. Budget observability accordingly, and log to files.

---

## Related

- [[Graph Engineering]] · [[Graph vs Workflow]] · [[Claude Code Graphs]] · [[Multi Agent Coding System]]
- [[Generator Evaluator Separation]] · [[Worktree Isolation]] · [[External State]]
- [[Source - Anthropic Building Effective Agents]] · [[Graph Engineering Origin and Fact-Check]]
