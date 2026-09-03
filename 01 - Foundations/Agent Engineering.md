---
title: Agent Engineering
aliases:
  - Building agents
tags:
  - agent-engineering
  - ai-engineering
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Agent Engineering

> [!abstract] One line
> The umbrella term. Not a layer of the stack but the whole activity: building systems where a
> model decides what to do next, and making that reliable.

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose. This version is grounded in the sources read
> on 2026-09-04.

---

## What an agent is `[FACT]`

The definition the field has converged on:

> **`Agent = Model + Harness`**

> "If you're not the model, you're the harness." — [[Source - Anatomy of an Agent Harness]]

> "An agent harness, also known as agent scaffolding, is the software infrastructure
> surrounding a large language model that enables it to operate as an AI agent."
> — [[Source - Wikipedia Agent Harness]]

`[FACT]` The split is older than the vocabulary: the UK AI Security Institute described an AI
agent as the model **plus scaffolding** in **2023**.

And Anthropic's operational definition, which is the one to keep:

> "**Agents** are systems where LLMs dynamically direct their own processes and tool usage,
> maintaining control over how they accomplish tasks" — as opposed to **workflows**, "where
> LLMs and tools are orchestrated through predefined code paths."

---

## What the job actually is `[FACT]`

The clearest job description in the literature, from OpenAI's report on building a product
with no hand-written code — the engineering team's work became:

> "**design environments, specify intent, and build feedback loops.**"

`[INFERENCE]` Three verbs, and none of them is "write code" or "write prompts." That is the
shift the discipline is named after. Note also that the triple maps onto three of the five
subsystems in [[Harness Components]] — environment, instructions, feedback — which is a
reasonable sanity check that the taxonomy is not arbitrary.

---

## The reflex that defines competence `[FACT]`

> "When something failed, the fix was almost never *try harder*… human engineers always
> stepped into the task and asked: **what capability is missing, and how do we make it both
> legible and enforceable for the agent?**"

Three organisations arrived at this independently. See [[Fix the Class Not the Instance]].

---

## The activity, layered

Agent engineering is not one of the layers in [[The Unified Mental Model]] — it spans them:

```text
GRAPH      how several agents and loops cooperate
LOOP       how one agent iterates to a goal
HARNESS    what it can do, see, remember, and be checked by
CONTEXT    what it knows this turn
PROMPT     how this turn is phrased
```

`[INFERENCE]` Anyone building agents does work at every level, whether or not they name it.
The value of the names is diagnostic: when something fails, they tell you **which layer to
look at**, cheapest first.

---

## The three principles worth memorising `[FACT]`

From [[Source - Anthropic Building Effective Agents]]:

> 1. Maintain **simplicity** in your agent's design.
> 2. Prioritize **transparency** by explicitly showing the agent's planning steps.
> 3. Carefully craft your **agent-computer interface (ACI)** through thorough tool
>    documentation and testing.

`[INFERENCE]` Principle 3 is the deepest and the least followed. The ACI is not just tool
schemas — it is every piece of text the agent reads: tool descriptions, error messages, file
names, log output, test names. Designing that text **is** agent engineering. See
[[Feedback Quality]].

---

## The discipline's central honesty problem `[INFERENCE]`

The component being engineered around is **non-deterministic**. [[Source - Wikipedia Agent Harness]]
names the consequence exactly: a harness "is designed to recover gracefully when the model
fabricates an action or **reports a task as finished when it is not**."

That is the difference from ordinary software engineering, and it is why so much of this vault
is about verification rather than capability. You are not building a system that is correct.
You are building a system that **notices when it is not**. See [[The Verification Gap]] and
[[Developer Tooling vs Harness Tooling]].

---

## Where to start

Not with a framework. `[FACT]` Anthropic: "start by using LLM APIs directly: many patterns can
be implemented in a few lines of code. If you do use a framework, ensure you understand the
underlying code."

Read [[GitHub - SWE-agent mini-swe-agent]] — a complete, competitive agent in about 100 lines.
Then [[Learning Roadmap]].

---

## Related

- [[Harness Engineering]] · [[Loop Engineering]] · [[Graph Engineering]] · [[The Unified Mental Model]]
- [[Agent Loops]] · [[Agent State]] · [[Agent Orchestration]] · [[Context Engineering]]
- [[Source - Anthropic Building Effective Agents]] · [[Agent Engineering MOC]]
