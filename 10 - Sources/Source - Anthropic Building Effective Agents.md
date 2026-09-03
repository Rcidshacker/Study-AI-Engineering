---
title: Source - Anthropic Building Effective Agents
aliases:
  - Building Effective Agents
  - The five workflow patterns
tags:
  - source
  - primary-source
  - agent-engineering
  - graph-engineering
  - anthropic
source-type: engineering-blog
author: Erik S. and Barry Zhang (Anthropic)
publisher: Anthropic
published: 2024-12-19
url: https://www.anthropic.com/engineering/building-effective-agents
reliability: high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Anthropic Building Effective Agents

> [!info] Verification
> Fetched and read directly on 2026-09-04. HTTP 200. Published **19 December 2024**, by
> **Erik S. and Barry Zhang**. The post now carries its own staleness notice: "Much of the
> tooling landscape described in this post has changed since December 2024." **The patterns
> have aged well; the tooling references have not.**

## Why it matters here

`[FACT]` Published **eighteen months before** "graph engineering" was named. Its five
workflow patterns, drawn out, are execution graphs of different shapes. It is the strongest
evidence for the claim in [[Graph Engineering Origin and Fact-Check]] that the practice
predates the vocabulary.

## The distinction that survived everything `[FACT]`

> - "**Workflows** are systems where LLMs and tools are orchestrated through **predefined code
>   paths**."
> - "**Agents** are systems where LLMs **dynamically direct their own processes and tool
>   usage**, maintaining control over how they accomplish tasks."

Both are "agentic systems." The dividing line is **who decides the path**. This is exactly the
node question in [[Graph vs Workflow]].

## The complexity discipline `[FACT]`

Stated three separate times in the post, which tells you how much they meant it:

> "We recommend finding the simplest solution possible, and only increasing complexity when
> needed. This might mean **not building agentic systems at all.**"

> "For many applications, optimizing single LLM calls with retrieval and in-context examples
> is usually enough."

> "Add multi-step agentic systems only when simpler solutions fall short."

`[INFERENCE]` Two years on this remains the most-ignored advice in the field, and it is the
backbone of [[When Not to Build a Harness]] and the caution in [[Claude Code Graphs]].

## On frameworks `[FACT]`

> "We suggest that developers **start by using LLM APIs directly**: many patterns can be
> implemented in a few lines of code. If you do use a framework, ensure you understand the
> underlying code. Incorrect assumptions about what's under the hood are a common source of
> customer error."

Frameworks "often create extra layers of abstraction that can obscure the underlying prompts
and responses, making them harder to debug."

`[INFERENCE]` The same reasoning is why this vault recommends reading
[[GitHub - SWE-agent mini-swe-agent]] and writing a shell loop yourself before adopting
built-in loop features.

## The building block: the augmented LLM `[FACT]`

An LLM plus retrieval, tools, and memory — which "can actively use these capabilities —
generating their own search queries, selecting appropriate tools, and determining what
information to retain." The post names **MCP** as one way to supply these augmentations.

This is `Agent = Model + Harness` before the word was in use. See
[[Lineage of the Word Harness]].

## The five patterns `[FACT]`

| Pattern | Shape | Use when |
|---|---|---|
| **Prompt chaining** | linear, with programmatic **gates** between steps | the task decomposes cleanly into fixed subtasks; trades latency for accuracy |
| **Routing** | classify, then dispatch to a specialist | distinct categories exist and classification is reliable. Named example: cheap model for easy questions, capable model for hard ones |
| **Parallelization** | fan-out, then aggregate | **sectioning** (independent subtasks) or **voting** (same task many times for confidence) |
| **Orchestrator-workers** | a central LLM decomposes, delegates, synthesises | you **cannot predict** the subtasks — the named example is coding, where the number of files to change is unknown up front |
| **Evaluator-optimizer** | generate ⇄ critique, in a loop | clear evaluation criteria exist and iterative refinement measurably helps |

Two details worth carrying forward:

- **Prompt chaining has "gates"** — programmatic checks between steps. `[INFERENCE]` A
  computational sensor inside a chain, years before [[Guides and Sensors]] named it.
- **Evaluator-optimizer is [[Generator Evaluator Separation]]**, and the post gives the fit
  test: it works when "LLM responses can be demonstrably improved when a human articulates
  their feedback" **and** "the LLM can provide such feedback." If a human could not usefully
  critique it, an LLM judge will not either.

## On agents proper `[FACT]`

> "They are typically just LLMs using tools based on environmental feedback **in a loop**."

The design requirements it names, all of which this vault treats as harness concerns:

- **Ground truth from the environment at each step** — "such as tool call results or code
  execution — to assess its progress." See [[The Verification Gap]].
- **Pause for human feedback** at checkpoints or blockers.
- **Stopping conditions**, "such as a maximum number of iterations, to maintain control."
  See [[Stopping Conditions]].
- **Sandboxed testing and guardrails**, because autonomy means "higher costs, and the
  potential for **compounding errors**." See [[Sandboxing and Permissions]].

## The three principles `[FACT]`

> 1. Maintain **simplicity** in your agent's design.
> 2. Prioritize **transparency** by explicitly showing the agent's planning steps.
> 3. Carefully craft your **agent-computer interface (ACI)** through thorough tool
>    documentation and testing.

`[INFERENCE]` Principle 3 is the underrated one and it is a direct antecedent of
[[Feedback Quality]]: tool descriptions and their output are an interface you design for a
reader that reasons. "Prompt engineering your tools" is the post's own phrase for it.

## How to use this source

- Read it **before** anything written about graphs in 2026. It is shorter, older, calmer, and
  covers most of the same ground.
- Treat the tooling references as historical; the post says so itself.
- Use the five patterns as a **vocabulary** for describing what you are building, not as a
  menu to pick from.

## Related

- [[Graph Engineering]] · [[Graph vs Workflow]] · [[Agent Orchestration]] · [[Agent Loops]]
- [[Generator Evaluator Separation]] · [[Graph Engineering Origin and Fact-Check]]
- [[Sources MOC]]
