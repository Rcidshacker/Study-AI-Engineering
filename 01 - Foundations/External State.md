---
title: External State
aliases:
  - Memory on disk
  - Cross-session state
tags:
  - harness-engineering
  - loop-engineering
  - agent-state
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# External State

> [!abstract] One line
> The model forgets everything between runs, so memory must live on disk. External state is
> not one component among several — it is the spine every loop and graph rests on.

---

## Why it is the foundation, not a feature

`[FACT]` [[Source - Wikipedia Agent Harness]]: rather than re-reading an ever-growing
transcript inside the context window, "a harness can offload record-keeping into a structured
software environment that manages the agent's state."

`[FACT]` [[Source - Addy Osmani Loop Engineering]] decomposes a loop into five building blocks
**plus** a memory layer — and the memory layer is explicitly *not* a peer of the other five.
It is what they all depend on.

`[FACT]` [[Source - Anthropic Effective Harnesses for Long-Running Agents]] gives the
metaphor: engineers working in shifts, each arriving with no memory of the previous shift.
The handover document is not a nicety in that scenario; it is the only thing that makes the
project possible.

---

## The three failed alternatives

| Approach | Why it fails |
|---|---|
| Keep everything in context | The window is finite; the cost is quadratic in attention terms and linear in money; you eventually hit the wall mid-task |
| Rely on compaction | `[FACT]` "Compaction isn't sufficient" — it "doesn't always pass perfectly clear instructions to the next agent." Summarisation is lossy in ways you cannot predict or inspect |
| Rely on session resume | Convenient, but tool-specific, not reviewable, not diffable, and not something a *different* agent or a human can pick up |

`[INFERENCE]` All three share one flaw: the state is **inside** something you cannot read or
edit. Externalising it is what makes it inspectable, correctable and portable.

---

## The four artefacts that work

Ordered by how much they carry.

### 1. Git history

`[FACT]` Anthropic: descriptive commits let the agent "revert bad code changes and recover
working states of the code base." Trivedy: "Git adds versioning to the filesystem so agents
can track work, rollback errors, and branch experiments."

`[INFERENCE]` Git is the only state store on this list that is also an **undo mechanism**.
That dual role is why it is first: it records what happened *and* bounds the damage of what
happened wrongly.

### 2. A structured status file

`feature_list.json` / `prd.json` — machine-checkable, append-only from the agent's side,
countable. See [[Feature List as Harness Primitive]].

### 3. A progress log

`claude-progress.txt` / `progress.txt` — narrative, appended never replaced. `[FACT]` Ralph's
instruction is explicit: *"APPEND to progress.txt (never replace, always append)."*

`[INFERENCE]` Append-only matters for the same reason immutable feature lists matter: a file
the agent may rewrite is a file the agent may quietly simplify into agreement with itself.

### 4. The repository itself

Design docs, architecture notes, execution plans. `[FACT]` "From the agent's point of view,
anything it can't access in-context while running effectively doesn't exist." See
[[The Repository as System of Record]].

---

## The tiering that makes it usable

Raw append-only logs grow past usefulness. `[FACT]` Ralph solves this with promotion:

```text
episodic   progress.txt body        what happened this run
   ↓ if general and reusable
semantic   ## Codebase Patterns     consolidated, at the TOP of the file, read first
   ↓ if durable
procedural nearby CLAUDE.md         permanent instruction
```

`[INFERENCE]` The placement detail is not incidental: patterns go at the **top** because a
fresh agent reads from the top and may not reach the bottom. **Ordering is part of the
interface.** The same reasoning explains why `AGENTS.md` should be a map — see
[[Context Window as a Budget]].

---

## Design rules

1. **Structured status in JSON; narrative in markdown.** `[FACT]` Models overwrite markdown
   more freely than JSON. Put the thing that must not be edited in JSON.
2. **Append, never replace**, for logs.
3. **The agent may change status, not scope.**
4. **Commit it.** State that is not in git is state you cannot review, diff, or roll back.
5. **Make the first read cheap.** A fresh session should learn where it is from three files,
   not thirty. See [[Coding Agent Startup Flow]].
6. **Prefer repo files to product features** for anything that must outlive a tool choice.

---

## The test

> Kill the session mid-task. Start a new one with no conversation history. Can it work out
> what was happening, what is done, what is next, and whether the tree is in a good state —
> from files alone?

`[INFERENCE]` If not, you do not have external state; you have a long conversation. Every
loop that runs unattended depends on the answer being yes, because that is precisely what
each iteration is.

---

## Related

- [[Agent State]] · [[The Repository as System of Record]] · [[Feature List as Harness Primitive]]
- [[Context Window as a Budget]] · [[Clean State Ritual]] · [[Ralph Loop]] · [[Loop Types]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]] · [[Source - Addy Osmani Loop Engineering]]
