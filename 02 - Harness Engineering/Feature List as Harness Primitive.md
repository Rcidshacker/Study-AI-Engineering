---
title: Feature List as Harness Primitive
aliases:
  - feature_list.json
  - Definition of done as an artefact
tags:
  - harness-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Feature List as Harness Primitive

> [!abstract] One line
> A machine-checkable list of every end-to-end behaviour, all starting `false`, that the
> implementing agent may **only** flip the status of. It is the cheapest harness component
> with the largest effect on long-running work.

---

## The shape `[FACT]`

From [[Source - Anthropic Effective Harnesses for Long-Running Agents]]:

```json
{
  "category": "functional",
  "description": "New chat button creates a fresh conversation",
  "steps": [
    "Navigate to main interface",
    "Click the 'New Chat' button",
    "Verify a new conversation is created",
    "Check that chat area shows welcome state",
    "Verify conversation appears in sidebar"
  ],
  "passes": false
}
```

Over **200** such entries for a claude.ai clone, written by an initializer agent from the
user's one-line prompt, all starting `false`.

---

## The four rules that make it work

### 1. Entries are **end-to-end and user-visible**

Not "the `ChatController` handles POST /messages" but "a user can open a new chat, type a
query, press enter, and see an AI response." `[INFERENCE]` The unit must be something a
**user** would notice is broken, because that is precisely the layer at which
[[The Verification Gap]] opens.

### 2. Entries carry **steps**, not just a title

The steps are what make an entry checkable by a browser-automation tool or a fresh agent
without re-deriving intent.

### 3. Everything starts **`false`**

`[FACT]` This is the direct fix for [[False Completion]]: it gives every later session "a
clear outline of what full functionality looked like." An empty backlog and a working app
look identical; a list of 200 `false` entries does not.

### 4. The implementing agent may change **only the status field** `[FACT]`

Enforced with deliberately strong language: *"It is unacceptable to remove or edit tests
because this could lead to missing or buggy functionality."*

`[INFERENCE]` This rule carries most of the weight. Without it, the shortest path to "all
green" is deletion — and an optimiser under a completion pressure will find it. The list is
only a constraint if it is **append-only from the agent's side**.

---

## Why JSON `[FACT]`

> "After some experimentation, we landed on using JSON for this, as **the model is less likely
> to inappropriately change or overwrite JSON files compared to Markdown files.**"

A rare, concrete, empirical file-format finding. `[INFERENCE]` Plausible mechanism: markdown
is the format models are trained to *rewrite freely*; JSON reads as data with a schema.
Whatever the cause, take the finding — the cost of following it is zero.

Secondary benefits: `jq` can compute progress, CI can gate on it, and a diff on a JSON status
change is unambiguous.

---

## What it gives you, beyond a to-do list

| Function | How |
|---|---|
| **Definition of done** | the loop's success condition is "every entry `true`" — [[Stopping Conditions]] |
| **Scope constraint** | "work on ONE entry per session" prevents one-shotting |
| **Cross-session state** | a fresh agent reads it and knows where it is — [[External State]] |
| **Progress metric** | `jq '[.[]|select(.passes)]|length'` — countable, no interpretation |
| **A verification script** | the `steps` are literally a test plan |

`[FACT]` [[Ralph Loop]] uses the identical structure (`prd.json`, user stories with `passes`)
and derives its stopping sentinel from it — an independent re-derivation of the same design,
which is good evidence the pattern is real.

---

## Where it comes from and where it goes

`[INFERENCE]` This is acceptance-criteria practice with two changes: it is **exhaustive up
front** (200 entries, not a sprint's worth) and it is **written by an agent from a one-line
prompt**. Both are affordable only because generation is cheap now. The old objection to
exhaustive up-front specification was that writing it cost more than it saved — that
arithmetic has changed.

The natural extension: entries whose `steps` are executable, so `passes` is computed rather
than asserted. At that point the feature list *is* the test suite, and
[[Generator Evaluator Separation]] comes free.

---

## Getting one

Ask an agent, in a dedicated first session, to expand your goal into an exhaustive list of
end-to-end behaviours with explicit steps, all `passes: false`, as JSON. Then **read it
yourself** — this is the one artefact worth reviewing by hand, because everything downstream
is measured against it. See [[Initialization as a Phase]].

---

## Related

- [[False Completion]] · [[The Verification Gap]] · [[Stopping Conditions]] · [[External State]]
- [[Initialization as a Phase]] · [[Ralph Loop]] · [[Autonomous Test Fixer]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]]
