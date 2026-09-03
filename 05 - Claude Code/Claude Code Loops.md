---
title: Claude Code Loops
aliases:
  - goal vs loop
  - Claude Code loop surfaces
tags:
  - claude-code
  - loop-engineering
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Claude Code Loops

> [!abstract] One line
> Claude Code ships **four distinct loop surfaces** — the built-in agentic loop, `/goal`,
> `/loop`, and Stop hooks — plus scheduled tasks and CI triggers around whole sessions. Pick
> by asking *what should start the next turn* and *what should stop it*.

> [!info] Verification
> Everything marked `[FACT]` below was read from the official Claude Code documentation at
> `code.claude.com/docs` on **2026-09-04** (`goal.md`, `features-overview.md`, and the docs
> index). Product surfaces move quickly — **re-read the docs before building on a command
> name.** The *shapes* are durable; the spellings are not.

---

## Layer 0 — the built-in agentic loop

The inner loop you do not control: Claude reasons, calls a tool, observes the result, and
repeats until it believes the turn is done. This is the [[Agent Loops]] ReAct pattern, and it
is [[Inner Harness vs Outer Harness|inner harness]]. Your influence over it is indirect —
through instructions, available tools, and what the tool results say. See
[[Claude Code Architecture]].

Everything below is about the loops **you** build around it.

---

## The three in-session loops `[FACT]`

The docs give a direct comparison, reproduced here because it is the clearest statement of
the design:

| Approach | Next turn starts when | Stops when |
|---|---|---|
| **`/goal`** | the previous turn finishes (or an idle check-in comes due while background work keeps the goal waiting, up to three times per goal between your prompts) | a model confirms the condition is met, **or judges it impossible**, or a turn fails on an error you have to fix, or you run `/goal clear` |
| **`/loop`** | a time interval elapses | you stop it, or Claude decides the work is done |
| **Stop hook** | the previous turn finishes | your own script or prompt decides |

### `/goal` — the goal loop `[FACT]`

> "The `/goal` command sets a completion condition and Claude keeps working toward it without
> you prompting each step. After each turn, **a small fast model checks whether the condition
> holds.** If the model judges it not yet met, Claude starts another turn instead of returning
> control to you."

Three things worth noticing, because they are exactly the design principles in
[[Loop Types]]:

1. **The judge is a separate model call**, not the working agent's own opinion. That is
   [[Generator Evaluator Separation]] built into the product.
2. **"Impossible" is a distinct terminal state** from "met." A loop that can only end in
   success is a loop that never ends. See [[Stopping Conditions]].
3. **Errors you must fix clear the goal.** The loop refuses to spin on a blocked condition.

Documented use cases: migrating a module until every call site compiles and tests pass;
implementing a design doc until all acceptance criteria hold; splitting a large file until
each part is under a size budget; working a labelled issue backlog until the queue is empty.

`[INFERENCE]` The pattern in all four: **a condition a machine can evaluate.** "Until the
code is good" is not a goal. "Until `pytest -x` exits 0 and `ruff check` is clean" is.

### `/loop` — the interval loop `[FACT]`

Time-triggered, and — critically — **each run is independent**. It does not resume where the
last one stopped. Use it to *watch* something, not to *build* something. See the
cumulative-vs-independent distinction in [[Loop Types]].

### Stop hooks — the programmable loop `[FACT]`

The most powerful of the three and the least used.

> "`/goal` and a Stop hook both fire after every turn. `/goal` is a session-scoped shortcut…
> A Stop hook lives in your settings file, applies to every session in its scope, and can run
> **a script for deterministic checks or a prompt for model-evaluated** [ones]."

| | `/goal` | Stop hook |
|---|---|---|
| Scope | this session only | every session in its settings scope |
| Defined | typed at the prompt | committed in settings |
| Check type | model judgment | **your script** (computational) or a prompt (inferential) |
| Version-controlled | no | yes |

`[INFERENCE]` This maps exactly onto [[Guides and Sensors]]: a Stop hook running your test
command is a **computational sensor** wired into the loop's exit condition; `/goal`'s judge is
an **inferential** one. A serious harness uses both — the script for what can be checked
deterministically, the model for what cannot.

---

## Loops around whole sessions `[FACT — features exist per docs index]`

| Surface | Loop type | Use |
|---|---|---|
| **Scheduled tasks** ("Run prompts on a schedule") | time-based | recurring chores, drift scans |
| **Routines** (Claude Code on the web) | time / event | automated work off your machine |
| **Channels** ("Push events into a running session") | event-driven | external systems waking a live session |
| **Headless** (`claude --print`) | anything you script | the substrate for [[Ralph Loop]]-style external loops |
| **GitHub Actions / GitLab CI** | event-driven | PR opened, CI failed, issue labelled |

`[INFERENCE]` **Headless mode is the one that matters most for learning.** Everything the
product offers as a feature, you can build yourself with `claude --print` inside a shell
loop — and building it once teaches you what the features are actually doing. That is exactly
what `ralph.sh` is. Start there, then adopt the built-ins.

---

## Choosing, in one page

```text
Does the work have an end state a machine can check?
├─ no  → don't build a loop yet. Define done first. → [[Feature List as Harness Primitive]]
└─ yes
   ├─ Should it run once, until done?
   │  ├─ this session, ad hoc            → /goal
   │  ├─ every session, versioned check  → Stop hook
   │  └─ fresh context each iteration,
   │     memory on disk, unattended      → external loop (headless + script) → [[Ralph Loop]]
   └─ Should it run repeatedly, forever?
      ├─ on a clock                      → /loop or scheduled task
      └─ on an external event            → channel, routine, or CI trigger
```

---

## What every one of these still needs from you

The loop surfaces are free; the things that make a loop *safe* are not. In every case you
must supply:

| Requirement | Where it comes from | Note |
|---|---|---|
| A checkable definition of done | `feature_list.json`, a test command, acceptance criteria | [[Feature List as Harness Primitive]] |
| A verification signal that is real | end-to-end tests, not just unit tests | [[The Verification Gap]] |
| An independent checker | a Stop-hook script, or a reviewer subagent | [[Generator Evaluator Separation]] |
| A budget | iteration cap, cost cap, wall-clock cap | [[Stopping Conditions]] |
| Isolation | worktree, branch, or container | [[Worktree Isolation]] |
| State that survives the context window | progress file, git, JSON status | [[External State]] |

`[INFERENCE]` A loop without the first four is not automation, it is an unattended way to
generate work you will have to review anyway. See [[Loop Failure Modes]].

---

## Related

- [[Loop Engineering]] · [[Loop Types]] · [[Stopping Conditions]] · [[Inner Loops and Outer Loops]]
- [[Ralph Loop]] · [[Autonomous Test Fixer]] · [[Claude Code Hooks]]
- [[Claude Code as a Harness]] · [[Claude Code Graphs]] · [[Claude Code MOC]]
