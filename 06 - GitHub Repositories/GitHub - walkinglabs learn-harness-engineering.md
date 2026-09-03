---
title: GitHub - walkinglabs learn-harness-engineering
aliases:
  - learn-harness-engineering
tags:
  - github
  - repository
  - harness-engineering
  - loop-engineering
  - graph-engineering
  - evergreen
repo: walkinglabs/learn-harness-engineering
url: https://github.com/walkinglabs/learn-harness-engineering
stars: 14741
license: MIT
language: TypeScript
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# GitHub — walkinglabs/learn-harness-engineering

## Overview

A fourteen-lecture course covering harness, loop **and** graph engineering in one framework,
with eight projects, runnable code per lecture, and copy-ready templates.

The content is analysed in [[Source - Learn Harness Engineering Course]]; **this note is about
the repository as an artefact** — what is in it and what to take.

## GitHub

`https://github.com/walkinglabs/learn-harness-engineering` — GitHub API, **2026-09-04**:

| Field | Value |
|---|---|
| Stars / forks | 14,741 / 1,470 |
| Language / licence | TypeScript / MIT |
| Created | 2026-03-29 |
| Last push | 2026-08-26 |
| Open issues | 11 |
| English docs | 128 files; 15 translations |

## Structure `[FACT — sparse clone, read 2026-09-04]`

```text
docs/en/lectures/          14 lectures, each with a code/ directory
docs/en/projects/          8 hands-on projects
docs/en/harness-designs/   breakdowns of Claude Code, Codex, Pi, DeepSeek
docs/en/resources/
   templates/              AGENTS.md, CLAUDE.md, claude-progress.md,
                           clean-state-checklist.md, evaluator-rubric.md,
                           feature_list.json, init.sh, session-handoff.md
   reference/              coding-agent-startup-flow, initializer-agent-playbook,
                           method-map, prompt-calibration
   openai-advanced/        a repo template mirroring the OpenAI docs/ layout, plus SOPs
skills/harness-creator/    the course packaged as a skill
tools/audit-harness.sh     a harness self-audit script
```

## Why it matters

`[INFERENCE]` Two things make it worth the disk space:

1. **It is the only source treating all three disciplines as one curriculum**, with a stated
   position on how they relate — which is the question [[The Unified Mental Model]] answers.
2. **The lecture titles are failure modes, not features.** "Why agents declare victory too
   early." "Why one giant instruction file fails." "Why end-to-end testing changes results."
   That is the right pedagogy for this field and mirrors the derivation method in
   [[Source - Anatomy of an Agent Harness]].

## The templates — the fastest thing to take `[FACT]`

`docs/en/resources/templates/` ships working versions of every artefact this vault recommends:
`feature_list.json`, `init.sh`, `claude-progress.md`, `session-handoff.md`,
`clean-state-checklist.md`, `evaluator-rubric.md`, and both `AGENTS.md` and `CLAUDE.md`.

`[INFERENCE]` If you take nothing else, take these. They are the shortest path from
[[Coding Agent Harness]] to a working setup, and they are MIT-licensed.

## What to study

1. **Lecture 02** — the five-subsystem model. See [[Harness Components]].
2. **Lecture 13** — loop types, and the `/goal` vs `/loop` distinction. See [[Loop Types]].
3. **Lecture 14** — graphs, **and its own fact-check of the graph hype**. See
   [[Graph Engineering Origin and Fact-Check]].
4. **`harness-designs/claude-code/`** — the course's reverse-engineering of Claude Code,
   read alongside [[Claude Code Architecture]] for the official account.
5. **`tools/audit-harness.sh`** — a self-audit you can run.

## Limitations

- `[CAUTION]` **Secondary source.** It synthesises the primaries; it does not report original
  work. Chase claims back before repeating them.
- `[CAUTION]` **It has an interest in its own framework.** There is a `dsh` plugin ecosystem
  and a `harness-creator` skill attached.
- `[UNVERIFIED]` Several dates and one case study I could not confirm — the 20%→100% staged
  study names no team or dataset, and its June 2026 date for Osmani's post did not match the
  live page. Details in [[Source - Learn Harness Engineering Course]].
- Product-surface claims (`/goal`, `/loop`, Routines) need checking against current docs.

## Related

- [[Source - Learn Harness Engineering Course]] · [[Harness Components]] · [[Loop Types]]
- [[Graph Engineering Origin and Fact-Check]] · [[Coding Agent Harness]] · [[Repository Index]]
