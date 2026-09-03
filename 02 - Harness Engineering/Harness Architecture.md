---
title: Harness Architecture
aliases:
  - How the pieces fit together
  - Harness Patterns
tags:
  - harness-engineering
  - architecture
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Harness Architecture

> [!abstract] One line
> How the five subsystems interact: instructions and environment shape what happens,
> **feedback closes the loop**, and state carries the result forward. Remove feedback and the
> other four become open-loop guessing.

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose. Component definitions live in
> [[Harness Components]]; this note is about the **interactions** between them.

---

## The circuit

```text
                    ┌──────────────────────────────────────┐
                    │            INSTRUCTIONS              │
                    │   what is true, what is forbidden,   │
                    │      where to look for more          │
                    └──────────────┬───────────────────────┘
                                   │ shapes intent
        ┌──────────────────────────▼──────────────────────────┐
   ┌───►│                       MODEL                          │
   │    └──────────────────────────┬──────────────────────────┘
   │                               │ emits an action
   │    ┌──────────────────────────▼──────────────────────────┐
   │    │   TOOLS  ──── act on ────►  ENVIRONMENT              │
   │    │   (what it can do)          (where it acts,          │
   │    │                              what is reproducible)   │
   │    └──────────────────────────┬──────────────────────────┘
   │                               │ produces an observable result
   │    ┌──────────────────────────▼──────────────────────────┐
   └────┤   FEEDBACK   tests · linters · types · judges        │
        │   ── the ONLY path by which being wrong becomes ──   │
        │   ──          information the model has         ──   │
        └──────────────────────────┬──────────────────────────┘
                                   │ outcome worth keeping
                    ┌──────────────▼───────────────────────┐
                    │               STATE                  │
                    │  git · progress file · status JSON   │
                    │  ──► read back at next session start │
                    └──────────────────────────────────────┘
```

`[INFERENCE]` Read the diagram as one claim: **only the feedback edge closes the circuit.**
Instructions, tools and environment are all feed-forward. A setup with four strong subsystems
and no feedback is not 80% of an agent harness; it is an open-loop system that produces
confident output nobody checks.

---

## The interactions that matter

### Instructions ⇄ Context budget

Every instruction costs context every session and **dilutes every other instruction**.
`[FACT]` "Too much guidance becomes non-guidance." So instructions and context are not
independent: making the instruction file longer makes it weaker.
Resolution: a map, not a manual. See [[Instruction File Design]].

### Instructions → Feedback (the promotion)

`[FACT]` OpenAI: "When documentation falls short, we **promote the rule into code**." A rule
that lives only in prose is advisory; the same rule as a lint check is enforced. See
[[Executable Rules Beat Written Rules]].

### Feedback → Instructions (the return path)

The non-obvious direction, and the highest-value one. `[FACT]` Both OpenAI and Thoughtworks
independently write **remediation instructions into their custom linter messages**. A sensor
delivering a guide, at the moment of the mistake, at zero cost when the code is clean. See
[[Feedback Quality]].

### Environment → Feedback

`[FACT]` What is verifiable depends on what the environment exposes. OpenAI made the app
bootable per worktree and wired in DOM access plus a per-worktree observability stack, so
agents query logs with LogQL and metrics with PromQL. That turns *"startup must complete in
under 800ms"* from a wish into a check.

`[INFERENCE]` The general rule: **any property you want maintained must be queryable by the
agent.** Otherwise it is aspiration. See [[Agent Observability]].

### State ⇄ Environment

A worktree without its gitignored files is a broken environment. `[FACT]` Claude Code's
`.worktreeinclude` exists for exactly this: worktrees are fresh checkouts, so `.env` and
friends are missing by default. Isolation and reproducibility are one problem, not two. See
[[Worktree Isolation]].

### Feedback → State

An outcome that is not recorded is re-derived. `[FACT]` Anthropic's session ritual reads
progress and git *before* touching new work, and runs a basic end-to-end check first — because
if the app was left broken, starting a new feature "would likely make the problem worse."

---

## The control taxonomy laid over the circuit

Every element above is one of four things `[FACT — Böckeler's framework]`:

|  | **Computational** | **Inferential** |
|---|---|---|
| **Guide** (before) | types · lockfiles · scaffolding · code mods · permission rules | instruction files · skills · examples |
| **Sensor** (after) | tests · linters · structural tests · builds | subagent review · LLM-as-judge |

`[FACT]` "Separately, you get either an agent that keeps repeating the same mistakes
(feedback-only) or an agent that encodes rules but never finds out whether they worked
(feed-forward-only)."

`[INFERENCE]` So the architecture requirement is not "have five subsystems." It is **have
something in all four cells.** An empty cell is a predictable pathology. See
[[Guides and Sensors]].

---

## Where the layers sit

The harness is the **substrate**; loops run on it; graphs coordinate loops:

```text
GRAPH     nodes, edges, shared state, routing
LOOP      trigger, act, verify, persist, stop
HARNESS   instructions · tools · environment · state · feedback
```

`[INFERENCE]` This ordering has a practical consequence people get backwards: **a loop cannot
supply a verification signal the harness lacks, and a graph cannot either.** Adding
orchestration on top of a weak feedback subsystem multiplies unverified output. Fix the
substrate first. See [[The Unified Mental Model]].

---

## Failure signatures by subsystem

| Symptom | Subsystem | Fix |
|---|---|---|
| Wrong package manager; ignores conventions | instructions | a short, specific instruction file |
| Stops and asks you to run things | tools | grant capability; least privilege, not no privilege |
| "Works on my machine"; parallel runs collide | environment | lockfiles, `init.sh`, worktrees |
| Re-solves solved problems | state | progress file, status JSON, git |
| **Ships broken code confidently** | **feedback** | **a real end-to-end check, then a hook that runs it** |

---

## Related

- [[Harness Components]] · [[Guides and Sensors]] · [[Harness Engineering]] · [[The Unified Mental Model]]
- [[Feedback Quality]] · [[Executable Rules Beat Written Rules]] · [[Agent Observability]]
- [[Coding Agent Harness]] · [[Claude Code as a Harness]]
