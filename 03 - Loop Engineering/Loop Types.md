---
title: Loop Types
aliases:
  - Types of agent loop
  - Turn-based goal-based time-based event-driven
tags:
  - loop-engineering
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Loop Types

> [!abstract] One line
> Loops differ by **what triggers them** and **what stops them**. Those two properties decide
> everything else; pick the wrong pair and the loop either never ends or never accumulates.

`[FACT]` The four-type taxonomy is from [[Source - Learn Harness Engineering Course]],
Lecture 13.

---

## The four types

| Type | Trigger | Stop condition | Best for |
|---|---|---|---|
| **Turn-based** | you type each prompt | agent thinks it's done, or you interrupt | small tasks, exploration |
| **Goal-based** | you state a goal once | an independent evaluator confirms done, or budget exhausted | complex tasks with a clear finish line |
| **Time-based** | a schedule (every N minutes/hours) | you stop it, or it exits after the work is gone | polling, periodic checks, recurring chores |
| **Event-driven** | an external event (PR opened, CI failed, issue filed) | event handled, or retry limit hit | reactive workflows, CI/CD integration |

These are **not competing designs**. A mature setup runs all four.

---

## The distinction people get wrong: cumulative vs. independent

`[FACT]` The course is emphatic about the difference between a goal loop and an interval loop,
because conflating them is the commonest beginner error:

| | Goal loop | Interval loop |
|---|---|---|
| Shape | one big task, runs until done | one small action, repeats |
| Stop | goal reached or budget spent | you stop it; or the task exits |
| Time | one long run — hours or days | short bursts on a schedule |
| **Progress** | **cumulative** — closer each iteration | **independent** — no accumulation |
| Analogy | a marathon: gun, then finish line | an alarm clock: rings, you turn it off |

> "A common mistake: shoving something that should be a goal into an interval loop… it runs
> the same instruction independently each time, it doesn't remember where it left off. You'll
> just get the same starting point over and over."

**The one-sentence test: does this thing have an end?**
Yes → goal loop. No, you just need to keep watching → interval loop.

`[INFERENCE]` The exception that rescues an interval loop is **external state**. An interval
loop *does* accumulate if each run reads and writes a shared progress artefact — that is
precisely how the [[Ralph Loop]] works, and why its "fresh context every iteration" is a
feature rather than a bug. Without external state, interval loops are memoryless. See
[[External State]].

---

## The four stages that produced the goal loop `[FACT]`

A useful history, because it explains *why* the design looks as it does:

1. **Manual one-by-one prompting.** You are the scheduler.
2. **Long multi-step prompts.** The agent runs several steps but drifts, and stalls at the end.
3. **Self-reflection.** The agent decomposes and retries on its own — but *when does it stop?*
   "Practice kept answering — no. Agents declare victory far too easily."
4. **Independent stopping judgment.** Take "is it done?" away from the agent doing the work
   and give it to a separate judge — another model, a script, or a test command.

> "The person writing the code can't grade their own homework."

Stage 4 is the whole point. See [[Generator Evaluator Separation]] and [[False Completion]].

---

## The three parts every loop has `[FACT]`

Whatever its type:

1. **A goal** — the end state, not the next step.
2. **A verification method** — how the end state is checked.
3. **A stopping condition** — including the budget for giving up.

> "More complex loops just add parts like scheduling, parallelism, isolation, and memory on
> top of these same three fundamentals."

If you cannot write all three down, you do not have a loop — you have a prompt that repeats.
See [[Stopping Conditions]].

---

## Loops in this vault

| Loop | Type | Note |
|---|---|---|
| [[Ralph Loop]] | goal (implemented as repeated fresh runs) | fresh context each iteration; memory in git + progress file |
| Maker/checker | goal | the minimum viable reliable loop |
| Test/fix | goal | stop = suite green; see [[Autonomous Test Fixer]] |
| Garbage collection | time-based | OpenAI's cadence of cleanup PRs; see [[Harness Debt and Garbage Collection]] |
| CI-failure handler | event-driven | stop = build green or retry limit |
| Research | goal | stop = questions answered *and* sources verified; see [[Research Agent]] |

---

> [!warning] Product surfaces change
> The course maps these types onto specific commands (`/goal`, `/loop`, Routines, thread
> automation). **Verify current command names against live documentation before building on
> them.** The taxonomy is durable; the command surface is not.

---

## Related

- [[Loop Engineering]] · [[Agent Loops]] · [[Inner Loops and Outer Loops]] · [[Stopping Conditions]]
- [[Ralph Loop]] · [[Generator Evaluator Separation]] · [[External State]]
- [[Source - Learn Harness Engineering Course]] · [[Source - Addy Osmani Loop Engineering]]
