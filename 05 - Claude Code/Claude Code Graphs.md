---
title: Claude Code Graphs
aliases:
  - Claude Code orchestration
  - Subagent graphs
tags:
  - claude-code
  - graph-engineering
  - agent-orchestration
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Claude Code Graphs

> [!abstract] One line
> Claude Code has a real graph layer — subagents, dynamic workflows, agent teams,
> cross-session messaging, worktrees — but **most tasks that look like graph problems are
> verification problems**, and you should rule that out first.

> [!info] Verification
> Feature existence and the quoted descriptions were read from the official documentation
> index and `features-overview.md` at `code.claude.com/docs` on **2026-09-04**. Behavioural
> claims beyond the quotes are marked `[INFERENCE]`.

---

## The graph primitives `[FACT]`

| Primitive | Documented as | Graph role |
|---|---|---|
| **Subagents** | "run their own loops in isolated context, returning summaries" | **node** with private context |
| **Dynamic workflows** | "run many subagents from a script Claude writes, returning one result" | **edges + routing**, expressed as code |
| **Agent teams** | "Orchestrate teams of Claude Code sessions" | long-lived parallel nodes |
| **Cross-session messaging** | "Claude passes a message from one of your sessions to another" | **edge** between running nodes |
| **Agent view** | "Manage multiple agents with agent view" | observability over the graph |
| **Worktrees** | "Run parallel sessions with worktrees" | the isolation that makes parallel nodes safe |
| **The filesystem / repo** | — | **shared state** |

Map that against the four parts of a graph in [[Graph Engineering]] — nodes, edges, shared
state, routing — and every part has a surface. The layer is genuinely present.

---

## The two properties that make a subagent a graph node `[FACT + INFERENCE]`

`[FACT]` "Isolated context, returning summaries." Two consequences, and they pull in
opposite directions:

1. **Context isolation is the feature.** A subagent can read forty files and return three
   sentences. The parent's context stays clean. This is [[Context Window as a Budget]] as an
   architectural move, and it is the *main* reason to use one.
2. **Context isolation is also the cost.** `[INFERENCE]` The subagent cannot see what the
   parent knows unless you put it in the prompt, and the parent only receives the summary.
   Information is lost at both boundaries. If the two need to share a lot, they should
   probably be one agent — or share a **file** rather than a message.

`[INFERENCE]` The design rule that follows: **subagents work best where the input is small,
the intermediate work is large, and the output is small.** Research, review, search, audit.
They work worst on tasks with rich shared context — a multi-file refactor where every
decision depends on every other.

---

## The one place a graph is unambiguously right

**Verification.** `[FACT]` The documented example for dynamic workflows is exactly this:
"Audit a whole codebase, with a second set of agents verifying each finding."

That shape — **produce in one node, verify in a different node with a fresh context** — is
[[Generator Evaluator Separation]], and it is a genuine structural fix, not a convenience.
A checker inside the same context shares the producer's assumptions and its blind spots.
As [[Source - Learn Harness Engineering Course]] puts it, a loop's checkpoints fail because
"the judge and the judged share one brain."

`[INFERENCE]` If you build exactly one graph in Claude Code, build this one:

```text
    ┌──────────────┐        ┌───────────────┐
    │ implement    │ diff   │ review        │  fresh context,
    │ (main agent) │───────►│ (subagent)    │  did not write the code
    └──────────────┘        └───────┬───────┘
           ▲                        │
           │  findings              │ verdict
           └────────────────────────┘
                shared state: the repo + a findings file
```

Wire it with a **Stop hook that runs a reviewer subagent** and you have a maker-checker loop
that fires automatically rather than when you remember. See [[Claude Code Hooks]] and
[[Claude Code Loops]].

---

## When a graph is *not* the answer

`[INFERENCE]` The honest hierarchy, cheapest first. Do not skip a rung:

| Symptom | Reach for | Not |
|---|---|---|
| Agent gets facts wrong | better context, a doc it can read | a research subagent |
| Agent's context fills up | move detail into skills and files | subagent fan-out |
| Agent ships broken code | **a test, and a hook that runs it** | a "tester agent" |
| Agent makes the same mistake repeatedly | a rule, a linter, a custom error message | a "reviewer agent" |
| One long task, one line of work | a loop | a graph |
| Genuinely independent parallel work, or a finding you must not trust | **a graph** | — |

> [!warning] The commonest expensive mistake
> Building a five-agent pipeline — planner, researcher, coder, tester, reviewer — for a task
> where the real problem is that `npm test` was never in `CLAUDE.md`. A graph multiplies your
> token spend, your latency, and your failure surface. It does not create a verification
> signal that does not exist. Fix [[The Verification Gap]] first, every time.
>
> Keep [[GitHub - SWE-agent mini-swe-agent]] in mind as the counter-example: a ~100-line
> **single-agent, single-loop** program reaching >74% on SWE-bench Verified.

---

## The four questions that tell you a loop has become a graph `[FACT]`

From [[Source - Learn Harness Engineering Course]], Lecture 14 — when these start coming up,
the loop has run out:

1. **Division of labour** — a research agent, an implementation agent, a testing agent: who
   goes first?
2. **Parallelism** — which parts can run at the same time?
3. **Rollback** — when tests fail, do you go back to implement, or all the way back to research?
4. **Handoff** — how do several agents see the same requirements, notes and results? If the
   reviewer disagrees with the implementer, who wins?

Question 4 is the one people underestimate. In Claude Code, the answer must be **the
filesystem**: a findings file, a status JSON, a progress log. Messages between agents are
lossy and unreviewable; files are neither. See [[External State]].

---

## Scaling notes `[INFERENCE]`

- **Worktrees before parallelism.** Two agents editing one working tree will corrupt each
  other's work. Isolation is a precondition, not an optimisation. See [[Worktree Isolation]].
- **Shared state must be a file, not a conversation.** It survives a crash, you can read it,
  and it can be committed.
- **Cross-session messaging is an edge, not a database.** `[FACT]` It is documented for
  "sessions you run yourself that need each other's findings mid-task" — coordination, not
  storage.
- **Observability scales worse than execution.** Five parallel agents are easy to start and
  hard to understand. Agent view exists for this; use it, and log to files.

---

## Related

- [[Graph Engineering]] · [[Graph vs Workflow]] · [[Agent Orchestration]] · [[Multi Agent Coding System]]
- [[Generator Evaluator Separation]] · [[The Verification Gap]] · [[Worktree Isolation]]
- [[Claude Code as a Harness]] · [[Claude Code Loops]] · [[Claude Code MOC]]
