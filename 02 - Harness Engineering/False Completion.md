---
title: False Completion
aliases:
  - Premature victory
  - Declaring victory too early
tags:
  - harness-engineering
  - failure-mode
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# False Completion

> [!abstract] One line
> The agent says it is done, and it is not. This is the defining failure mode of long-running
> coding agents, and it is a **structural** problem, not a discipline problem.

---

## The canonical description `[FACT]`

From [[Source - Anthropic Effective Harnesses for Long-Running Agents]]:

> "After some features had already been built, a later agent instance would look around, see
> that progress had been made, and declare the job done."

Note the mechanism carefully. The agent is not lying and it is not lazy. It arrives with **no
memory of the plan**, observes a codebase that clearly works, and reaches the only reasonable
conclusion available from what it can see. **The information that would contradict "done" did
not exist in its environment.**

That is why exhortation fails. "Be thorough" does not add information.

---

## The three ingredients

False completion needs all three. Remove any one and it stops.

| Ingredient | Removed by |
|---|---|
| No durable definition of "all of done" | a feature list — [[Feature List as Harness Primitive]] |
| No independent judge | [[Generator Evaluator Separation]] |
| Verification too weak to contradict the claim | [[The Verification Gap]] |

`[FACT]` The third is the subtle one. Anthropic's agents *were* testing — unit tests, curl
against a dev server — and still "failed to recognize that the feature didn't work end-to-end."
**Weak verification is more dangerous than none**, because it manufactures confidence.

---

## The documented fixes

### 1. A comprehensive, immutable-except-status feature list `[FACT]`

An initializer agent expands the prompt into every end-to-end behaviour — **over 200 entries**
for a claude.ai clone — all starting `"passes": false`. Coding agents may change **only** the
status field, reinforced with: *"It is unacceptable to remove or edit tests because this could
lead to missing or buggy functionality."*

`[INFERENCE]` The immutability rule is doing most of the work. Without it, the cheapest path
to "all green" is to delete the red entries — and that is exactly what an optimiser does.

### 2. JSON over Markdown `[FACT]`

Anthropic: "the model is less likely to inappropriately change or overwrite JSON files
compared to Markdown files." A small, concrete, empirical finding worth respecting.

### 3. Verify as a user would `[FACT]`

Browser automation, driving the real application. OpenAI went further: the agent **records
video** of the failure before the fix and of the resolution after it.

### 4. Check the state before starting `[FACT]`

Every Anthropic session begins by running `init.sh`, starting the dev server, and doing a
basic end-to-end pass **before** touching a new feature — because if the app was left broken,
starting new work "would likely make the problem worse." See [[Coding Agent Startup Flow]].

---

## The general principle `[INFERENCE]`

> **"Done" must be a property of the repository, not a belief of the agent.**

If the only artefact recording completion is the agent's message, you have no completion
signal — you have a claim. Make it a file, make it checkable, and make something other than
the author check it.

---

## Detecting it in your own setup

Ask, in order:

1. Is there a file that lists **everything** that must be true when this is finished?
2. Can a **command** evaluate each entry?
3. Can the agent that implements an entry **edit or delete** it? (If yes: fix this first.)
4. Does anything other than the implementing agent set the status?
5. Does the session **verify the existing state** before adding new work?

`[INFERENCE]` A "no" at step 1 or a "yes" at step 3 means false completion is not a risk in
your setup — it is a certainty on a long enough run.

---

## Related

- [[The Verification Gap]] · [[Generator Evaluator Separation]] · [[Feature List as Harness Primitive]]
- [[Clean State Ritual]] · [[Harness Failure Modes]] · [[Loop Failure Modes]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]]
