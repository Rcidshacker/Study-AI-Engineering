---
title: Fix the Class Not the Instance
aliases:
  - The steering loop
  - Harness iteration
  - Permanent fixes
tags:
  - harness-engineering
  - practice
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Fix the Class Not the Instance

> [!abstract] One line
> When the agent gets something wrong, do not correct the output — change the environment so
> that class of mistake becomes unlikely or impossible. This single reflex is what separates
> harness engineering from prompting.

---

## The same reflex, described three times independently `[FACT]`

| Source | Formulation |
|---|---|
| [[Source - OpenAI Harness Engineering]] | "When something failed, the fix was almost never *try harder*… human engineers always stepped into the task and asked: **what capability is missing, and how do we make it both legible and enforceable for the agent?**" |
| [[Source - Harness Engineering for Coding Agent Users]] | "Whenever an issue happens **multiple times**, the feedforward and feedback controls should be improved to make the issue less probable to occur in the future, or even prevent it." |
| [[Source - Wikipedia Agent Harness]] (reporting Mitchell Hashimoto) `[UNVERIFIED]` | engineering "a permanent fix into an agent's environment each time it makes a mistake" |

`[INFERENCE]` Three independent arrivals at the same practice, from a frontier lab, a
consultancy and an individual practitioner. In a field this noisy, that is the strongest
evidence available that something is real.

---

## The trigger is **repetition**, not severity

Böckeler's phrasing is precise: *whenever an issue happens multiple times.* `[INFERENCE]` This
is the correct threshold and it is easy to get wrong in both directions:

- **Too eager** — a control for every one-off mistake produces the bloated instruction file
  that [[Source - OpenAI Harness Engineering]] documents as failing: "when everything is
  'important,' nothing is."
- **Too slow** — you correct the same thing in chat forever, paying the cost every session and
  never banking the fix.

The working rule: **first time, correct it in the conversation. Second time, note it. Third
time, build the control.**

---

## The escalation ladder

When you decide to fix the class, pick the *weakest* control that actually holds. Stronger
controls cost more and constrain more.

| # | Control | Type | Cost | Strength |
|---|---|---|---|---|
| 1 | a line in `CLAUDE.md` | inferential guide | trivial | weak — advisory, and competes for context |
| 2 | a skill loaded on demand | inferential guide | low | weak, but free when not triggered |
| 3 | a better error message on an existing check | **inferential guide inside a computational sensor** | low | **strong — best value on this ladder** |
| 4 | a new test or lint rule | computational sensor | medium | strong |
| 5 | a structural/architecture test | computational sensor | medium | strong, catches whole classes |
| 6 | a hook that blocks the action | computational guide | medium | absolute, within its scope |
| 7 | a permission rule | computational guide | low | absolute, blunt |

`[INFERENCE]` **Level 3 is the sweet spot and the most underused.** You usually already have
the check; you are simply not using its output as a teaching surface. See
[[Feedback Quality]].

Level 1 is where everyone starts and it is the weakest thing on the list: it is advisory, it
is inferential, and it costs context every single session whether relevant or not. A rule that
can be violated silently is not a control. See [[Executable Rules Beat Written Rules]].

---

## Promote, don't accumulate `[FACT]`

The knowledge has to move upward, or the instruction file becomes a graveyard.
[[Ralph Loop]] implements a clean three-tier promotion:

```text
observation      →   appended to progress.txt      (episodic, this run)
   ↓ if general and reusable
pattern          →   ## Codebase Patterns, at the TOP of progress.txt
   ↓ if durable
instruction      →   a nearby CLAUDE.md            (permanent)
```

with an explicit rule against promoting story-specific detail or debugging notes.

`[INFERENCE]` The promotion *criterion* is the important part: **generality**. Something is
worth encoding only if it will apply to work you have not thought of yet. Everything else
belongs in the run log.

---

## The agent builds its own controls `[FACT]`

Böckeler: agents "can help write structural tests, generate draft rules from observed
patterns, scaffold custom linters, or create how-to guides from codebase archaeology."

OpenAI's custom linters were "Codex-generated, of course."

`[INFERENCE]` This changes the economics decisively. The historical objection to bespoke
static analysis was that writing a custom lint rule cost more than the bugs it caught. When
the agent writes the rule in five minutes from three examples of its own mistakes, the
calculation inverts — and the *reason* to write it is now stronger too, because the rule
teaches every future agent run, not just future humans.

---

## The failure this prevents

Without this reflex you get an agent that is corrected constantly and **improves never**, and
a human whose job has quietly become full-time output review. `[INFERENCE]` That is the
real cost: not the individual mistakes, but that none of the corrections accumulate anywhere.
The harness is the place corrections accumulate.

---

## Related

- [[Guides and Sensors]] · [[Feedback Quality]] · [[Executable Rules Beat Written Rules]]
- [[Harness Debt and Garbage Collection]] · [[Instruction File Design]] · [[Ralph Loop]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Harness Engineering for Coding Agent Users]]
