---
title: Graph Engineering
aliases:
  - Agent Graphs
  - Graph-based agent systems
tags:
  - graph-engineering
  - agent-orchestration
  - evergreen
status: evergreen
confidence: medium
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Graph Engineering

> [!abstract] One line
> What a loop becomes when the work needs specialisation, parallelism, shared state and
> independent verification. The practice is real and old; the name is about two months old and
> arrived as a joke.

> [!warning] Read the fact-check before anything else
> The term went viral off a deliberate parody, and several of the numbers circulating with it
> are fabricated. Full account, with what is and is not verified:
> [[Graph Engineering Origin and Fact-Check]].

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose that treated this as a settled discipline.

---

## Is it an established term?

`[INFERENCE]` **No.** Established more weakly than [[Harness Engineering]], which is itself
only a year old:

- Named around **July 2026**, from a joke tweet satirising the industry's term cycle.
- One genuine precursor survives fact-checking (Josh Simmons, 4 July 2026) `[UNVERIFIED]`.
- No agreed taxonomy, no textbook, no widely-adopted repository implementing it as a *named*
  discipline. See the honest gap note in [[Repository Index]].
- The underlying practice, by contrast, has been documented since **December 2024**.

Use the word if it is useful. Do not treat it as a body of knowledge.

---

## The four parts `[FACT]`

Strip it down and a graph is:

| Part | What it is |
|---|---|
| **Node** | a unit of work with a responsibility — deterministic code, a model call, a tool, **or a full agent with its own loop** |
| **Edge** | how work hands off: sequence, parallelism, conditionals, retry-into-self, **rollback to a node several hops back** |
| **Shared state** | the data package all nodes read and write. "Nodes don't shout at each other; they all read and write the same state" |
| **Routing** | the control flow: tests pass → ship; tests fail → back to implement; not enough information → back to research |

`[INFERENCE]` The **rollback edge** is the part a single loop cannot express explicitly. In a
loop, "go back and reconsider" happens inside one context window, invisibly, as a decision the
agent makes and you cannot inspect. In a graph it is a declared edge.

---

## What actually distinguishes it from workflow orchestration `[FACT]`

The instinct that this is just DAGs is **half right**. Graphs and workflows share the same
skeleton, and Airflow, Prefect, Dagster and Temporal have orchestrated it for years.

**The difference is in what a node may be.** A traditional workflow node is a deterministic
function with hardcoded `if`/`switch` edges: the same input always walks the same path. A
graph node may be a full agent that reasons, uses tools, and retries on its own.

That maps exactly onto Anthropic's older distinction: workflows are "orchestrated through
predefined code paths"; agents "dynamically direct their own processes." See
[[Graph vs Workflow]].

---

## Why a loop eventually needs one `[FACT]`

Four questions that a single loop cannot answer, from Lecture 14:

1. **Division of labour** — who goes first?
2. **Parallelism** — what runs at once?
3. **Rollback** — on failure, back to which node?
4. **Handoff** — how do agents share state, and who wins a disagreement?

And three **structural** failures that in-loop checkpoints cannot fix, because "the judge and
the judged share one brain and one context":

- **Goodhart** — a metric pushed hard stops measuring what it did. The cited case: a loop
  optimising ticket-resolution rate while churn doubled, because "the bot learned to close
  tickets."
- **Blindness upward** — a loop cannot ask whether its target is the right target. "A
  thermostat can't ask whether 68°F is the right temperature."
- **Conflict** — independently built loops undermine each other while each dashboard looks
  healthy.

> "A graph doesn't give you more checkpoints; it **moves the check** — from inside the agent to
> a standalone node with a fresh context."

That is the one genuinely structural argument for a graph. See
[[Generator Evaluator Separation]].

---

## Anchors — the part everyone skips `[FACT]`

However elegant your network of loops, if every loop drifts from reality the network is "just
a resonance of mutual drift." An **anchor** pins a loop to the world: real business outcomes,
ground-truth datasets, human spot-checks.

`[INFERENCE]` Anchors are [[The Verification Gap]] at graph scale. Same disease, larger
organism — and the same fix: something outside the system that can say *no*.

---

## The honest trade `[FACT]`

> "Loops have a lot of room for forgiveness. Graphs force you to admit how much of your
> workflow is not actually modeled." — attributed to Luis Catacora `[UNVERIFIED]`

> "A loop is a **deferred decision** … A graph is an **up-front decision**. You must declare
> the whole structure in advance: who owns what, how tasks depend on each other, where a given
> failure returns to. It's more work — and it buys you readability, auditability, and local
> repair."

> "A loop hides the problem inside the loop; a graph puts the problem on paper. The former
> suits exploration, the latter suits production."

---

## When *not* to build one

`[INFERENCE]` The commonest expensive mistake is a five-agent pipeline for a project whose
real problem is that the test command was never written down. A graph multiplies token spend,
latency and failure surface; it does **not** create a verification signal that does not exist.

Keep [[GitHub - SWE-agent mini-swe-agent]] in view: single agent, single loop, ~100 lines,
>74% on SWE-bench Verified. Full decision table in [[Claude Code Graphs]].

---

## Related

- [[Graph Engineering Origin and Fact-Check]] · [[Graph vs Workflow]] · [[Claude Code Graphs]]
- [[Agent Orchestration]] · [[Generator Evaluator Separation]] · [[The Unified Mental Model]]
- [[Source - Anthropic Building Effective Agents]] · [[Source - Learn Harness Engineering Course]]
