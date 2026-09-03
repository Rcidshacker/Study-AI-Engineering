---
title: Graph vs Workflow
aliases:
  - Is graph engineering just workflows
  - Workflow Engineering
tags:
  - graph-engineering
  - comparison
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Graph vs Workflow

> [!abstract] One line
> Same skeleton, different nodes. A workflow node is a deterministic function; a graph node may
> be an agent that decides for itself. Everything else follows from that.

---

## The instinct is half right `[FACT]`

Anyone with production experience says the same thing when they meet "graph engineering":
*isn't this just workflows?* DAGs, state machines, and workflow engines have existed for
decades.

> "**That instinct is half right.** Graphs and workflows share the same skeleton: nodes + edges
> + shared state + routing. Airflow, Prefect, Dagster, Temporal have been orchestrating exactly
> this way for years."

`[FACT]` And Anthropic's five patterns from December 2024 — prompt chaining, routing,
parallelisation, orchestrator-workers, evaluator-optimizer — "when drawn out, are precisely
execution graphs of different shapes."

---

## The half that is wrong `[FACT]`

> "**The half that's wrong is in the nodes.** Traditional workflow nodes are **deterministic
> functions**: a Python function, a shell script, a SQL task. The edges are hardcoded code:
> `if`, `switch`, `case`. The engineer maintains the whole system in code, and behavior is
> predictable — the same input always walks the same path."

A graph node may instead be "a full agent — with its own loop, able to understand goals, use
tools, and retry on its own."

Which lines up exactly with Anthropic's older distinction `[FACT]`:

> - **Workflows** — "LLMs and tools are orchestrated through **predefined code paths**."
> - **Agents** — "LLMs **dynamically direct their own processes** and tool usage."

**Who decides the path** is the whole question.

---

## What changes when a node can decide

| | Workflow node | Agent node |
|---|---|---|
| Same input, same path? | yes | **no** |
| Failure modes | enumerable | open-ended |
| Debugging | read the code | read the transcript |
| Retry | re-run the function | it may retry *internally*, invisibly |
| Testing | unit-testable | needs evals — [[Agent Evaluation]] |
| Cost | predictable | varies per run |
| Handles the unforeseen | no | **yes** — the entire point |

`[INFERENCE]` The trade is legibility for adaptability, and it is genuinely a trade. A
deterministic node you can reason about is worth a great deal. **Make a node an agent only
where you actually need it to handle cases you did not enumerate** — which, in practice, is
fewer nodes than people build.

---

## The mixed graph is usually right `[INFERENCE]`

Nothing requires every node to be the same kind. The strongest designs in the sources are
**mixed**:

```text
  research   ── agent      (open-ended: cannot predict what it must read)
     ↓
  implement  ── agent      (open-ended: cannot predict the change)
     ↓
  verify     ── CODE       (deterministic: run the suite, exit 0 or not)
     ↓ fail ──────────────► back to implement
     ↓ pass
  merge      ── CODE       (deterministic: commit, update state)
```

`[FACT]` Anthropic's prompt-chaining pattern already includes this idea — programmatic
**"gates"** between LLM steps.

`[INFERENCE]` The rule: **verification nodes should be deterministic wherever the property is
checkable.** A test runner is a perfect judge within its scope, costs nothing per run, and
never flatters. Reserve agent-judges for what tests cannot express. See
[[Generator Evaluator Separation]] and [[Guides and Sensors]].

---

## When to use which

| Situation | Build |
|---|---|
| The steps are known and fixed | **workflow** — cheaper, debuggable, testable |
| The steps are known but a step needs judgement | **mixed**: code edges, one agent node |
| You cannot predict the subtasks | **agent nodes** — Anthropic's stated criterion for orchestrator-workers |
| You need a finding checked by something that did not produce it | **graph**, with a fresh-context node |
| One goal, one line of work | **neither** — a loop |

---

## The honest summary `[INFERENCE]`

"Graph engineering" is not a new discipline. It is **workflow orchestration where nodes may be
non-deterministic**, which is a real and consequential change to a mature field, and not the
invention of one. Treat the decades of workflow engineering as applicable prior art — because
it is — and treat the non-determinism as the part that genuinely needs new thinking, mostly
around verification, retry semantics, and cost.

See [[Graph Engineering Origin and Fact-Check]] for how the term arrived, and why some of what
is written about it is fabricated.

---

## Related

- [[Graph Engineering]] · [[Agent Orchestration]] · [[Claude Code Graphs]]
- [[Generator Evaluator Separation]] · [[Agent Evaluation]] · [[The Unified Mental Model]]
- [[Source - Anthropic Building Effective Agents]] · [[Source - Learn Harness Engineering Course]]
