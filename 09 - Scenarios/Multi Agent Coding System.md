---
title: Multi Agent Coding System
aliases:
  - Multi Agent Development
  - Scenario 7
  - Planner researcher coder tester reviewer
tags:
  - scenario
  - graph-engineering
  - agent-orchestration
  - evergreen
status: evergreen
confidence: medium
created: 2026-09-04
updated: 2026-09-04
---

# Scenario — Multi-Agent Coding System

> [!abstract] The task
> Planner, researcher, coder, tester, reviewer. How do they communicate, and who controls the
> loop?

> [!warning] Answer the prior question first
> `[INFERENCE]` This is the most-requested and least-often-justified architecture in agent
> work. Before building it, confirm you are not solving [[The Verification Gap]] with
> org-chart cosplay. A five-agent pipeline on a project with no reliable test command produces
> five times the unverified output. The counter-example:
> [[GitHub - SWE-agent mini-swe-agent]] — **one agent, one loop, ~100 lines, >74% on SWE-bench
> Verified.**

---

## Which roles survive scrutiny

`[INFERENCE]` Judged by whether a separate agent buys something a single loop cannot:

| Role | Justified? | Why |
|---|---|---|
| **Reviewer** | **yes — structurally** | A fresh context that did not write the code. The only role with a reason no in-loop checkpoint can supply — [[Generator Evaluator Separation]] |
| **Researcher** | **yes — economically** | Reads forty files, returns three paragraphs. Isolation is the product — [[Context Window as a Budget]] |
| **Tester** | **usually no** | If tests exist, run them; a test runner is a better judge than an agent. Justified only for *writing* tests, or exploratory testing a suite cannot express |
| **Planner** | **often no** | A plan is an artefact, not an agent. Write `feature_list.json` once; the plan then lives on disk where everyone reads it |
| **Coder** | n/a | This is the main agent |

The honest reduction: **coder + reviewer, with a researcher when context demands it.** That is
Anthropic's evaluator-optimizer plus occasional sectioning — see
[[Source - Anthropic Building Effective Agents]] — and it covers most of the value.

---

## How they communicate

`[INFERENCE]` The single most important design decision, and the one people get wrong.

| Channel | Survives a crash | Reviewable | Diffable | Use for |
|---|---|---|---|---|
| **Shared files in the repo** | ✅ | ✅ | ✅ | **everything that matters** |
| Return values / summaries | ❌ | ❌ | ❌ | one-shot handoff of a small result |
| Direct messaging | ❌ | ❌ | ❌ | coordination signals only |

`[FACT]` Claude Code's cross-session messaging is documented for "sessions you run yourself
that need each other's findings mid-task" — coordination, **not storage**.

So the shared state is a directory:

```text
work/<task-id>/
├── plan.md          written by planning, read by everyone
├── research.md      researcher's findings, with sources
├── findings.json    reviewer's output: {file, line, claim, severity, verdict}
└── status.json      the single source of truth on what is done
```

`[INFERENCE]` This also answers the conflict question. **If the reviewer disagrees with the
implementer, the reviewer wins** — but only because the disagreement is written down in
`findings.json` where a human can overrule it. An unrecorded disagreement resolved by whoever
spoke last is not a decision, it is an accident.

---

## Who controls the loop

`[INFERENCE]` Three options, in increasing order of how much you should have to justify them:

| Control | Shape | Use when |
|---|---|---|
| **A script** | deterministic edges; agents are nodes | the sequence is known — **start here** |
| **A hook** | reviewer fires automatically at a lifecycle event | you want verification without asking |
| **An orchestrator agent** | a model decides who runs next | `[FACT]` Anthropic's criterion: "you can't predict the subtasks needed" |

`[FACT]` Claude Code's dynamic workflows are "a script Claude writes that runs many subagents"
— the middle ground, with the documented example being exactly the shape worth building:
"audit a whole codebase, with a **second set of agents verifying each finding**."

**Prefer deterministic edges.** A node that must decide is a node you must debug by reading a
transcript. See [[Graph vs Workflow]].

---

## A design that holds up

```text
                    ┌──────────────────────────────┐
   shared state ───►│  work/<task-id>/  (the repo) │◄─── every node reads & writes
                    └──────────────────────────────┘
   research (agent, isolated) ──plan──► implement (main agent)
                                             │ diff
                                             ▼
                              review (subagent, FRESH context)
                                    │              │
                            findings│              │clean
                                    ▼              ▼
                               implement       tests (CODE, deterministic)
                                                   │ green
                                                   ▼
                                            merge (CODE)
```

Rules: **agents where judgement is needed, code where it is not**; every node reads and writes
files; the reviewer never wrote the code; a budget on the review↔implement cycle so it cannot
loop forever — see [[Stopping Conditions]].

---

## The four costs, stated plainly `[INFERENCE]`

1. **Tokens** multiply by node count, and the reviewer re-reads context the implementer already
   paid for.
2. **Latency** — sequential nodes serialise; parallel nodes need [[Worktree Isolation]].
3. **Debuggability collapses.** Five transcripts, and the failure is usually in the handoff
   rather than in any one of them. See [[Agent Observability]].
4. **Information is lost at every boundary.** Isolation is the feature and the cost.

`[FACT]` Anthropic names the general risk: agent autonomy brings "higher costs, and the
potential for **compounding errors**."

---

## Build it in this order

1. Main agent + **tests**. Stop here unless something is still failing.
2. Add a **reviewer subagent**, fired by a `Stop` hook. Most of the value, one extra node.
3. Add a **researcher** only when context pressure is the actual complaint.
4. Add an **orchestrator** only when you genuinely cannot predict the subtasks.

`[INFERENCE]` Most teams should stop at step 2 and would be better off spending the remaining
effort on [[The Verification Gap|the tests]].

---

## Related

- [[Agent Orchestration]] · [[Claude Code Graphs]] · [[Graph vs Workflow]] · [[Graph Engineering]]
- [[Generator Evaluator Separation]] · [[External State]] · [[Worktree Isolation]]
- [[Source - Anthropic Building Effective Agents]] · [[Scenarios MOC]]
