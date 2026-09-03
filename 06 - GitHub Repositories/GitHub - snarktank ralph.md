---
title: GitHub - snarktank ralph
aliases:
  - snarktank/ralph
  - ralph repo
tags:
  - github
  - repository
  - loop-engineering
  - claude-code
  - evergreen
repo: snarktank/ralph
url: https://github.com/snarktank/ralph
stars: 21700
license: MIT
language: TypeScript
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# GitHub — snarktank/ralph

## Overview

An autonomous agent loop that runs a coding tool (Amp or [[Claude Code]]) **repeatedly until
all PRD items are complete**. Each iteration is a fresh instance with clean context; memory
lives in git history, `progress.txt` and `prd.json`.

The concept note is [[Ralph Loop]]; this note is about the repository.

## GitHub

`https://github.com/snarktank/ralph` — verified via the GitHub API **2026-09-04**:

| Field | Value |
|---|---|
| Stars / forks | 21,700 / 2,089 |
| Language / licence | TypeScript / MIT |
| Created | 2026-01-07 |
| Last push | **2026-02-02** |
| Open issues | 74 |

> [!note] Maintenance status
> Last push **2026-02-02**, with 74 open issues, is roughly seven months stale as of
> verification. `[INFERENCE]` Read it as a **reference implementation of a pattern**, not as
> maintained software. The pattern is what has spread; the shell script is ~120 lines and you
> should expect to own your copy.

## Why it matters

`[FACT]` The README credits **Geoffrey Huntley's Ralph pattern** (`ghuntley.com/ralph/`) as
its basis. `[FACT]` [[Source - OpenAI Harness Engineering]] independently names the same
pattern — their PR-completion loop "effectively is a Ralph Wiggum Loop." A pattern named by
an independent practitioner and then cited by a frontier lab's engineering report is about as
strong a community-practice signal as this field offers.

## Architecture

```text
ralph.sh              the loop: iterate, watch for the completion sentinel, budget out
prompt.md             the per-iteration prompt for Amp
CLAUDE.md             the per-iteration prompt for Claude Code
prd.json.example      the work definition — user stories with a `passes` flag
skills/prd/           a skill that generates a PRD
skills/ralph/         a skill that converts a PRD to prd.json
.claude-plugin/       Claude Code marketplace packaging
flowchart/            diagrams of the loop
```

`[INFERENCE]` The structure is itself a lesson: the *loop* is a shell script, the *policy* is
a markdown file, and the *work* is JSON. Three different artefact types for three different
rates of change — the script almost never changes, the policy occasionally, the JSON every
iteration. See [[Harness Components]].

## Important files

| File | Read it for |
|---|---|
| `ralph.sh` | budget, sentinel detection, failure tolerance, run archiving |
| `CLAUDE.md` | the ten-step per-iteration contract and the stop condition |
| `prd.json.example` | the `passes: false` work-definition shape |
| `skills/prd`, `skills/ralph` | how to package this as installable Claude Code skills |

## Harness engineering lessons

- **Instructions as a re-read file, not a growing conversation.** The same `CLAUDE.md` is
  piped in every iteration. Policy is stable; only state moves.
- **Three-tier memory promotion.** Episodic notes appended to `progress.txt` → general
  patterns promoted to a `## Codebase Patterns` section at the *top* of that file → durable
  knowledge written into nearby `CLAUDE.md` files. The repo is explicit that only "general
  and reusable" items are promoted, and that story-specific detail must not reach `CLAUDE.md`.
  `[INFERENCE]` This is the most transferable idea here: a working
  [[Fix the Class Not the Instance]] loop inside the agent loop.
- **Clean-state ritual at the run boundary.** Changing branch archives the previous
  `prd.json`/`progress.txt` to a dated folder and resets the log. See [[Clean State Ritual]].

## Loop engineering lessons

- **A hard iteration budget** (`MAX_ITERATIONS`, default 10) with a **distinct failure exit**
  (`exit 1`) — running out of budget is never reported as success.
- **`|| true` on the agent invocation** — one failed iteration must not kill the loop.
- **A sentinel string as the completion signal**: `<promise>COMPLETE</promise>`, emitted only
  when every story has `passes: true`. Machine-checkable, not vibes.
- **One story per iteration**, enforced in the prompt — the direct counter to the
  "one-shotting" failure in
  [[Source - Anthropic Effective Harnesses for Long-Running Agents]].

## Graph engineering lessons

None. Ralph is a **single loop, single agent** and gets a long way on that alone. `[INFERENCE]`
Its weakest point is precisely the one a graph would fix: the agent that implements a story
is also the agent that sets `passes: true`. See [[Generator Evaluator Separation]].

## Claude Code relevance

Directly usable: skills installable via the plugin marketplace, and a `CLAUDE.md` you can
adapt. Read it beside
[[Source - Anthropic Effective Harnesses for Long-Running Agents]] — Ralph is essentially an
independent re-derivation of the same design (feature list + progress file + git + one
feature per session), which is why it is worth studying.

## Limitations

- `[FACT]` Runs `claude --dangerously-skip-permissions` (and `amp --dangerously-allow-all`).
  **Do not run it unsandboxed on a repo you care about.** See [[Sandboxing and Permissions]].
- Self-graded completion, as above.
- Requires `jq`, a git repo, and an authenticated CLI.
- Stale since February 2026.

## What to study

1. `ralph.sh` — specifically the budget, the sentinel, and `|| true`.
2. `CLAUDE.md` — the ten steps, and how the stop condition is phrased.
3. The `Codebase Patterns` promotion rule; then add it to your own progress files.
4. Then ask: *where would I insert an independent checker?*

## Related

- [[Ralph Loop]] · [[Loop Types]] · [[Stopping Conditions]] · [[External State]]
- [[Feature List as Harness Primitive]] · [[Autonomous Test Fixer]] · [[Repository Index]]
