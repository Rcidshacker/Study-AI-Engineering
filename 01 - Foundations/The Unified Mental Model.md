---
title: The Unified Mental Model
aliases:
  - Harness Loop Graph relationship
  - The four-layer stack
  - Is the sibling model correct?
tags:
  - ai-engineering
  - agent-engineering
  - harness-engineering
  - loop-engineering
  - graph-engineering
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# The Unified Mental Model

> [!abstract] One line
> Harness, loop and graph are **not siblings** — they are **layers in a stack**, and the
> stack is *cumulative*: each layer keeps the ones below it rather than replacing them.

This note answers the question posed directly: *is the proposed model correct?* It is not,
and the way it is wrong is instructive.

---

## The proposed model

```text
                    AGENT SYSTEM
                         │
             ┌───────────┼───────────┐
             │           │           │
          HARNESS       LOOP        GRAPH
             │           │           │
          Context     Feedback     Routing
          Tools       Verify       State
          Rules       Retry        Dependencies
          Safety      Improve      Relationships
```

**The column contents are largely right. The topology is wrong.**

Three specific problems:

1. **It makes the three coordinate.** They are not. A loop runs *inside* a harness — it uses
   the harness's tools, environment and feedback channels. A graph is what a loop becomes
   when the work needs specialisation. You cannot pick "graph" *instead of* "harness."
2. **It assigns feedback and verification to the loop only.** Feedback is a harness
   subsystem — see [[Harness Components]]. The loop *consumes* feedback; it does not supply
   it. If your tests don't exist, no amount of loop design creates a verification signal.
3. **It omits the two layers underneath.** Prompt and context engineering did not go away.
   Every iteration of every loop still assembles a prompt and a context.

---

## The corrected model

```text
   ┌─────────────────────────────────────────────────────────┐
   │  GRAPH        many agents/loops: nodes, edges,           │  the system
   │               shared state, routing, veto & rollback     │
   ├─────────────────────────────────────────────────────────┤
   │  LOOP         one goal, run until met: trigger,          │  the runtime
   │               act, verify, persist, stop                 │
   ├─────────────────────────────────────────────────────────┤
   │  HARNESS      instructions · tools · environment ·       │  the substrate
   │               state · feedback   (Agent = Model+Harness) │
   ├─────────────────────────────────────────────────────────┤
   │  CONTEXT      what the model sees this turn              │  the information
   ├─────────────────────────────────────────────────────────┤
   │  PROMPT       how we phrase this turn's instruction      │  the instruction
   └─────────────────────────────────────────────────────────┘
                      MODEL  (not yours to engineer)
```

Read it as **cumulative, not successive**:

- Discovering context engineering did not stop anyone prompting. The loop just refreshes the
  prompt each turn as the environment moves.
- Building loops did not remove context work. Every round of a loop **reassembles** its context.
- At the graph layer, all of it survives: **every node has its own prompt, its own context,
  its own tools, its own memory, and its own loop.** The graph only decides how nodes connect.

`[FACT]` The prompt → context → loop → graph progression is documented in
[[Source - Learn Harness Engineering Course]] (Lecture 14), which credits a thread by
@rohit4verse. `[INFERENCE]` The insertion of **harness** beneath loop is the course's
correction, and I agree with it — see the disagreement section below, because it is not settled.

---

## Where each layer lives, concretely

| Layer | Question it answers | What you actually build | Claude Code surface |
|---|---|---|---|
| Prompt | How do we phrase it? | wording, examples, output format | the message you type; skill bodies |
| Context | What should it know now? | what's loaded, what's excluded, what's compacted | `CLAUDE.md`, file reads, MCP resources, compaction |
| **Harness** | What can it do, see, and be checked by? | instructions, tools, environment, state, feedback | `CLAUDE.md`, `.claude/`, permissions, hooks, MCP, tests |
| **Loop** | How does it iterate until done? | trigger, work discovery, verify, persist, stop | the built-in agent loop; scripted outer loops; scheduled runs |
| **Graph** | How do many of these cooperate? | nodes, edges, shared state, routing | subagents + shared files + a driver script |

See [[Claude Code as a Harness]], [[Claude Code Loops]], [[Claude Code Graphs]].

---

## The one genuine disagreement in the literature `[FACT]`

Where **harness** sits is *not settled*, and honest notes should say so:

| Position | Source | Claim |
|---|---|---|
| Harness **contains** context engineering | [[Source - Wikipedia Agent Harness]] | "the harness designs the whole operational environment and contains the other two as parts" |
| Harness **is a form of** context engineering | [[Source - Harness Engineering for Coding Agent Users]] | "Engineering a user harness for a coding agent is a specific form of context engineering" |
| Harness sits **above** the loop | explainx (as cited by the course) | — |
| Harness sits **below** the loop | the Buildrix paper (as cited by the course) | — |

`[INFERENCE]` My reading, and the one this vault uses: these describe **different cuts of
the same object rather than a factual dispute**.

- Böckeler is right *mechanically*: the way you deliver guides and sensors to the agent is by
  putting things in its context. Delivery is context engineering.
- Wikipedia is right *architecturally*: the harness includes things that are not context at
  all — a sandbox, a permission boundary, a filesystem, a test runner.

So: **context engineering is the harness's delivery mechanism; the harness is larger than
what it delivers.** Both containments hold along different axes, which is why the argument
does not resolve. Do not treat either as settled fact.

---

## The other terms

**Agent engineering** is the umbrella, not a layer. `Agent = Model + Harness`, so anyone
building agents is doing prompt, context, harness, loop and possibly graph work. See
[[Agent Engineering]].

**Workflow engineering** is the *predecessor* of the graph layer, and it is genuinely
different — the difference is in what a node is allowed to be. See
[[Graph vs Workflow]]. Traditional workflow nodes are deterministic functions with
hardcoded `if`/`switch` edges; graph nodes may be full agents with their own loops.

**Developer tooling** is the substrate the harness reuses. See
[[Developer Tooling vs Harness Tooling]] — the distinguishing property is that a harness
wraps a **non-deterministic** component and must recover gracefully when the model fabricates
an action or claims a task is finished when it is not.

---

## How to use the stack in practice

The layers are also a **debugging order**. When an agent fails, ask in this sequence — the
answers are cheapest at the bottom:

1. **Prompt** — was the instruction ambiguous? (cheapest fix, and often it is this)
2. **Context** — did it have the information? Was the relevant file even in context?
3. **Harness** — could it *see* it was wrong? Was there a test, a linter, a type check?
   Did it have the tool it needed? Was the environment reproducible?
4. **Loop** — did it stop too early, retry forever, or never get triggered?
5. **Graph** — did the wrong specialist take the work? Did two agents fight over one file?

`[INFERENCE]` Most teams that think they have a graph problem have a **layer-3 problem**.
The commonest real cause of "the agent keeps getting it wrong" is that nothing in the
environment can tell it that it is wrong. Fix the feedback subsystem before you draw a graph.
See [[The Verification Gap]] and [[When Not to Build a Harness]].

---

## What is actually *new* about all this `[INFERENCE]`

Loops, graphs, feedback control and state machines are decades old. What changed:

- The unit of work is **non-deterministic**, so the harness must be designed for recovery
  rather than for correctness-by-construction.
- The unit of work is **cheap and parallel**, so throughput can exceed human review capacity —
  which changes merge philosophy, review economics and the value of mechanical enforcement.
- The unit of work **reads its own environment**, so the environment becomes a programming
  surface. Writing a linter error message is now prompt engineering.

That third point is the genuinely novel one. See [[Executable Rules Beat Written Rules]].

---

## Related

- [[Harness Engineering]] · [[Loop Engineering]] · [[Graph Engineering]]
- [[Harness Components]] · [[Inner Loops and Outer Loops]] · [[Graph vs Workflow]]
- [[Context Engineering]] · [[Prompt Engineering vs Context Engineering]]
- [[Harness vs Loop vs Graph]] · [[Harness Loop Graph MOC]]
