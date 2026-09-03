---
title: Source - Anatomy of an Agent Harness
aliases:
  - Trivedy 2026
  - LangChain agent harness
tags:
  - source
  - primary-source
  - harness-engineering
  - langchain
source-type: engineering-blog
author: Vivek Trivedy (LangChain)
publisher: LangChain
published: 2026-03-10
url: https://blog.langchain.com/the-anatomy-of-an-agent-harness/
reliability: medium-high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Anatomy of an Agent Harness

> [!info] Verification
> Fetched and read directly on 2026-09-04. HTTP 200. By-line **Vivek Trivedy, March 10,
> 2026**, on the LangChain blog, stated 12-minute read.
>
> `[BIAS NOTE]` LangChain sells agent infrastructure (LangGraph, deepagents, LangSmith,
> Sandboxes). The component list below maps closely onto their product surface. The
> reasoning is sound and the derivation method is genuinely useful, but read the
> *component inventory* as a vendor's view of the space, not a neutral taxonomy.

## The definition `[FACT]`

> "Agent = Model + Harness. **If you're not the model, you're the harness.**"

> "A harness is every piece of code, configuration, and execution logic that isn't the model
> itself. A raw model is not an agent. But it becomes one when a harness gives it things like
> state, tool execution, feedback loops, and enforceable constraints."

And the framing sentence worth memorising:

> "The model contains the intelligence and **the harness makes that intelligence useful**."

## The derivation method — the actual contribution `[FACT]`

The article's real value is not its component list but its **method**. Rather than
enumerating parts, it works backwards:

```text
Behaviour we want (or want to fix)  →  Harness design that achieves it
```

Every component is justified by a capability the model lacks. Out of the box a model
cannot:

- maintain durable state across interactions
- execute code
- access real-time knowledge
- set up environments and install packages

> "The main idea is that we want to convert a desired agent behaviour into an actual feature
> in the harness."

`[INFERENCE]` This is the single most transferable idea in the piece, and it is the correct
way to build *your* harness: never add a component because a list says to. Name the failure
you observed, then design the minimal control that removes it. It is the same reflex as
[[Fix the Class Not the Instance]], applied at design time rather than repair time.

## The component inventory `[FACT — as stated]`

> "Concretely, a harness includes things like:
> - System Prompts
> - Tools, Skills, MCPs — and their descriptions
> - Bundled Infrastructure (filesystem, sandbox, browser)
> - Orchestration Logic (subagent spawning, handoffs, model routing)
> - Hooks/Middleware for deterministic execution (compaction, continuation, lint checks)"

Compare against Böckeler's guides/sensors 2×2 and the course's five subsystems in
[[Harness Components]] — the three inventories overlap but are cut along different axes.

## The filesystem as the foundational primitive `[OPINION — well argued]`

> "The filesystem is arguably the most foundational harness primitive."

The argument, which is good:

1. Models were trained on **billions of tokens of filesystem usage**, so the affordance is
   already native. No new interface has to be taught.
2. Work can be **incrementally offloaded** instead of held in context.
3. It is a **natural collaboration surface** — multiple agents and humans coordinate through
   shared files.
4. **Git adds versioning** on top: agents can track work, roll back errors, and branch
   experiments.

`[INFERENCE]` Point 1 is the deep one and it generalises: **prefer harness interfaces the
model has already seen millions of times** (files, bash, git, JSON, markdown) over bespoke
APIs it must be taught in-context. This is the same reasoning that leads
[[Source - OpenAI Harness Engineering]] to favour "boring" technologies. See
[[The Filesystem as Harness Primitive]].

## Bash as the general-purpose tool `[FACT]`

> "Instead of forcing users to build tools for every possible action, a better solution is to
> give agents a general purpose tool like bash."

The stated reason harnesses need this: the execution pattern is a **ReAct loop** — reason,
act via tool call, observe, repeat — but "harnesses can only execute the tools they have
logic for." Bash breaks that ceiling: "the model can design its own tools on the fly via code
instead of being constrained to a fixed set of pre-configured tools."

See [[Agent Loops]] and [[Tool Design for Agents]].

## Sandboxes `[FACT]`

Stated rationale: running agent-generated code locally is risky, and a single local
environment does not scale to large agent workloads. See [[Sandboxing and Permissions]].

## How to use this source

- Take the **derivation method**, and the **filesystem/bash/git** arguments — these are
  general.
- Discount the orchestration-and-middleware emphasis proportionally to LangChain's
  commercial interest in exactly those layers.
- Cross-check the component list against
  [[Source - Harness Engineering for Coding Agent Users]], which arrives at a different cut
  from a vendor-neutral position.

## Related

- [[Harness Engineering]] · [[Harness Components]] · [[The Filesystem as Harness Primitive]]
- [[Source - Wikipedia Agent Harness]] · [[Sources MOC]]
