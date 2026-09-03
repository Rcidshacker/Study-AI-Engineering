# Study AI Engineering

An Obsidian vault on **harness engineering, loop engineering, and graph engineering** for AI
coding agents, with a working focus on Claude Code.

**Start at [[AI Engineering MOC]].**

---

## What this is

Research notes built from primary sources, each fetched and read rather than summarised from
memory. The organising idea:

> `Agent = Model + Harness`. You cannot change the model. Everything you *can* change is the
> harness, the loops that run on it, and the graphs that coordinate those loops.

---

## Structure

```text
00 - MOC/                 maps of content — start here
01 - Foundations/         agents, loops, state, context
02 - Harness Engineering/ the substrate: instructions, tools, environment, state, feedback
03 - Loop Engineering/    the runtime: goals, verification, stopping conditions
04 - Graph Engineering/   the system: nodes, edges, shared state, routing
05 - Claude Code/         verified against official docs
06 - GitHub Repositories/ repos, verified against the GitHub API
07 - Practical Examples/  reproducible configurations
08 - Comparisons/         how the terms relate
09 - Scenarios/           worked situations
10 - Sources/             the evidence base, the glossary, the roadmap
```

Two folders are **not** part of the knowledge base:

- `_QUARANTINE_fabricated_2026-09-04/` — five notes containing invented repositories, kept as
  evidence. Read its README before anything else in this vault.
- `_ARCHIVE_unverified_2026-09-03/` — first-pass drafts, superseded.

---

## Evidence standard

Every claim carries a tag:

| Tag | Meaning |
|---|---|
| `[FACT]` | stated in a primary source I read directly |
| `[PRACTICE]` | widely-reported community practice |
| `[OPINION]` | a named person's position, attributed |
| `[INFERENCE]` | my own synthesis — reasoning, not reporting |
| `[UNVERIFIED]` | reported somewhere, could not confirm |
| `[CAUTION]` | verified to exist, with a reliability or bias caveat |

Rules the vault follows:

1. **Claims live in source notes; concept notes cite source notes.** One place to correct each
   fact.
2. **Every source note carries a `verified:` date.** Treat anything months old as stale.
3. **Unverifiable claims are marked, not dropped.** The standing list is in [[Sources MOC]].

---

## Why the quarantine folder exists

An earlier pass of this research produced **twenty fabricated GitHub repositories** with
invented star counts, presented as verified fact. They were caught by checking every URL
against the GitHub API — a check that took under a minute and should have run first.

The fabricated notes are kept, committed, and documented rather than deleted. A record of a
caught failure is worth more than the appearance of never having failed, and it is this
vault's own worked example of [[The Verification Gap]] — the subject it is about.

Write-up: [[Research Integrity in Agent-Assisted Research]].

---

## The six load-bearing claims

| Claim | Confidence |
|---|---|
| The environment explains more outcome variance than model choice, on long tasks | medium-high |
| Most agent unreliability reduces to the agent being unable to tell it was wrong | high |
| The thing that did the work must not decide the work is done | high |
| Anything the agent reads is a prompt, including error messages | high |
| Harness, loop and graph are layers, not alternatives | medium-high |
| Fix the class of failure, not the instance | high |

---

## Where to start reading

1. [[The Unified Mental Model]] — the frame, with the common diagram corrected
2. [[The Verification Gap]] — the thing everything else depends on
3. [[Coding Agent Harness]] — build something in ninety minutes
4. [[Learning Roadmap]] — the eight levels, each with an exit test
5. [[Sources MOC]] — what is verified, and what is not
