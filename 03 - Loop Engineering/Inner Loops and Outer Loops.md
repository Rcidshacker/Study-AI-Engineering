---
title: Inner Loops and Outer Loops
aliases:
  - Own the outer loop
  - Inner loop vs outer loop
tags:
  - loop-engineering
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Inner Loops and Outer Loops

> [!abstract] One line
> The inner loop is what the agent does inside one turn; the outer loop is what you build
> around whole sessions. **The inner loop is the vendor's product. The outer loop is your
> job.**

---

## The two loops

| | Inner loop | Outer loop |
|---|---|---|
| Scope | one session / one turn | across sessions |
| Cycle | reason → act → observe → repeat | trigger → discover work → dispatch → verify → persist → repeat |
| Owner | the agent vendor | **you** |
| Ends when | the agent thinks the turn is done | a stopping condition you defined |
| Memory | the context window | files, git, issue trackers |
| Changeable by you | only indirectly | completely |

`[FACT]` The inner loop is the ReAct pattern — "a model reasons, takes an action via a tool
call, observes the result, and repeats in a while loop"
([[Source - Anatomy of an Agent Harness]]). You can read one in ~30 lines in
[[GitHub - SWE-agent mini-swe-agent]].

`[FACT]` Addy Osmani's post *Own the Outer Loop* (15 Jul 2026, date verified) is the piece
that most directly names this split. See [[Source - Addy Osmani Loop Engineering]].

---

## Why the distinction matters

`[INFERENCE]` It tells you **where to spend effort**, and the answer is counter-intuitive to
most people's instincts.

You can influence the inner loop only indirectly — through instructions, available tools, and
what tool results say. You cannot change how it decides to stop, how it plans, or how it
compacts. Attempts to micro-manage it produce the bloated instruction files that
[[Source - OpenAI Harness Engineering]] documents as failing.

You own the outer loop completely: what triggers it, what work it picks up, what counts as
verified, what it does on failure, when it gives up, and what it leaves behind. Every
documented reliability gain in this vault lives there.

`[INFERENCE]` It also mirrors [[Inner Harness vs Outer Harness]] exactly, and for the same
reason: the vendor ships the part that runs, you build the part that governs. Outer-loop work
**transfers between tools**; inner-loop knowledge does not.

---

## The human's position

`[FACT]` This is the whole content of [[Loop Engineering]] as Osmani defines it: *"replacing
yourself as the person who prompts the agent."*

```text
BEFORE — you are the outer loop
  you → prompt → agent → output → you inspect → you prompt again → …
  Your throughput is the system's throughput.

AFTER — you built the outer loop
  trigger → discover work → dispatch → verify → persist → next
                                                     └→ escalate → you
  You define the goal and stopping condition before, review after,
  and adjust the rules when it drifts.
```

`[FACT]` The course's framing of the same shift: leverage moves from "writing the right
prompt" to "designing the right loop."

---

## Where each Claude Code surface sits `[FACT — surfaces per official docs, 2026-09-04]`

| Surface | Loop |
|---|---|
| The built-in agentic loop | **inner** |
| `/goal` | outer, in-session — an outer loop the product runs for you |
| Stop hook | outer, in-session, **versioned** |
| `/loop`, scheduled tasks | outer, across turns, time-triggered |
| Channels, routines, CI triggers | outer, across sessions, event-triggered |
| Headless + a shell script | outer, entirely yours — see [[Ralph Loop]] |

`[INFERENCE]` `/goal` and Stop hooks are interesting precisely because they are outer-loop
control **injected into the inner loop's boundary** — they fire after every turn, which is the
one seam the vendor exposes. That is why they are the highest-leverage loop surfaces
available.

---

## The four silent costs of running outer loops `[FACT — as reported by the course, from Osmani]`

Loops accelerate output *and* risk. The named costs, which grow the longer a loop runs:

1. **Verification debt** — output accumulating faster than anyone confirms it works.
2. **Comprehension rot** — a codebase nobody on the team has read.
3. **Cognitive surrender** — approving because checking is tiring.
4. **Token blowout** — cost scaling with iterations, not with value.

`[INFERENCE]` Costs 1 and 3 are the same failure at different layers, and they are the reason
[[Generator Evaluator Separation]] is non-negotiable in an outer loop: if the only reviewer is
a tired human, the loop's effective verification approaches zero as its throughput rises.
Cost 2 is what OpenAI's golden principles and garbage-collection loops exist to fight — see
[[Harness Debt and Garbage Collection]].

---

## Building your first outer loop

Start where the judge already exists: [[Autonomous Test Fixer]]. Then add, in order — a
trigger, work discovery from a status file, an independent verifier, state persistence, and a
budget. Each addition should be forced by an observed failure, not by this list.

---

## Related

- [[Loop Engineering]] · [[Loop Types]] · [[Agent Loops]] · [[Stopping Conditions]]
- [[Inner Harness vs Outer Harness]] · [[Claude Code Loops]] · [[Ralph Loop]]
- [[Source - Addy Osmani Loop Engineering]]
