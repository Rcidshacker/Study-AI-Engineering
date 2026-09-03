---
title: Clean State Ritual
aliases:
  - Coding Agent Startup Flow
  - Initialization as a Phase
  - Session lifecycle
tags:
  - harness-engineering
  - agent-state
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Clean State Ritual

> [!abstract] One line
> A session's first act is to find out where it is; its last act is to leave the place fit for
> the next one. Both ends need a written ritual, because neither happens by default.

Covers the three phases of a session's lifecycle: **initialisation** (once), **startup**
(every session), and **clean state** (every session end).

---

## Phase 1 — Initialisation, which deserves its own prompt `[FACT]`

Anthropic's design uses **two different agents**:

| Agent | Runs | Job |
|---|---|---|
| **Initializer** | first session only, with a *different prompt* | create `init.sh`, `claude-progress.txt`, a comprehensive feature list, and a baseline git commit |
| **Coding agent** | every session after | one feature, then structured updates and a clean state |

`[FACT]` The "different prompt for the very first context window" pattern was already
recommended in Anthropic's Claude 4 prompting guide.

`[INFERENCE]` The reason it must be separate: initialisation is the only session where *nothing
exists yet*, so the instructions that make every other session productive — read the progress
file, pick the next feature — are meaningless. Trying to serve both with one prompt gets you a
first session that improvises the scaffolding it will later depend on.

**Review the initializer's output by hand.** It is the one artefact everything downstream is
measured against. See [[Feature List as Harness Primitive]].

---

## Phase 2 — Startup, every session `[FACT]`

The documented ritual:

1. Run `pwd` — "you'll only be able to edit files in this directory."
2. Read the **git log** and **progress file** to get up to speed.
3. Read the **feature list**; choose the highest-priority item not yet done.
4. Run `init.sh` to start the dev server, **then run a basic end-to-end check before
   implementing anything new.**

Step 4 is the one people omit, and the rationale is precise:

> If the app had been left broken and the agent "had instead started implementing a new
> feature, it would likely make the problem worse."

`[INFERENCE]` Two failures at once: you lose the ability to attribute the breakage, and you
compound it. Verifying first turns every session into a clean experiment — the same reasoning
that makes one-feature-at-a-time work.

`[FACT]` Ralph's version adds a memory-ordering detail: read the **`## Codebase Patterns`
section at the top of the progress file first**, before the run log. Consolidated knowledge
before episodic detail.

**Automate it.** A `SessionStart` hook that runs `init.sh` and prints the progress file plus
the next unfinished feature turns this from an instruction the agent might follow into
something that has already happened.

---

## Phase 3 — Clean state, every session end `[FACT]`

The definition is precise and worth quoting rather than paraphrasing:

> "By 'clean state' we mean **the kind of code that would be appropriate for merging to a main
> branch**: there are no major bugs, the code is orderly and well-documented, and in general, a
> developer could easily begin work on a new feature **without first having to clean up an
> unrelated mess**."

Mechanisms:

- **Commit to git with descriptive messages** — which also gives the agent the ability to
  "revert bad code changes and recover working states."
- **Write a progress summary** — appended, never replacing.

`[FACT]` Ralph adds a run-boundary ritual: changing branch archives the previous `prd.json` and
`progress.txt` to a dated folder and resets the log.

---

## Why the ritual is necessary at all `[FACT]`

> "Imagine a software project staffed by engineers working in shifts, where each new engineer
> arrives with no memory of what happened on the previous shift."

`[INFERENCE]` Under that constraint, the handover document is not politeness — it is the only
mechanism by which the project has continuity. And unlike human shift handover, there is no
possibility of asking. Whatever was not written down is gone.

This is also why the two failure modes it prevents are the two that Anthropic names:
**one-shotting** (leaving a feature half-done and undocumented) and
[[False Completion|premature victory]] (a later agent seeing progress and declaring done).

---

## The checklist

**Session start**
- [ ] Where am I? (`pwd`, branch)
- [ ] What happened before? (git log, progress file — patterns section first)
- [ ] What is next? (feature list, highest priority not done)
- [ ] **Does it currently work?** (`init.sh`, then an end-to-end check)

**Session end**
- [ ] Checks pass
- [ ] Committed, with a descriptive message
- [ ] Progress appended: what was done, files touched, learnings
- [ ] Reusable patterns promoted; status field updated
- [ ] Nothing half-finished left behind

---

## Related

- [[Agent State]] · [[External State]] · [[Feature List as Harness Primitive]] · [[False Completion]]
- [[Claude Code Hooks]] · [[Ralph Loop]] · [[Autonomous Test Fixer]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]]
