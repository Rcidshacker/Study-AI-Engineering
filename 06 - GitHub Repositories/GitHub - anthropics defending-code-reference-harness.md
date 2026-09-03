---
title: GitHub - anthropics defending-code-reference-harness
aliases:
  - defending-code-reference-harness
  - Anthropic reference harness
tags:
  - github
  - repository
  - harness-engineering
  - claude-code
  - security
  - evergreen
repo: anthropics/defending-code-reference-harness
url: https://github.com/anthropics/defending-code-reference-harness
stars: 7403
license: NOASSERTION
language: Python
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# GitHub — anthropics/defending-code-reference-harness

## Overview

Anthropic's own reference implementation of an autonomous pipeline: Claude Code skills for
threat modelling, scanning, triage and patching, plus a runnable harness that does the whole
loop unattended.

## GitHub

`https://github.com/anthropics/defending-code-reference-harness` — GitHub API, **2026-09-04**:

| Field | Value |
|---|---|
| Stars / forks | 7,403 / 597 |
| Language / licence | Python / NOASSERTION |
| Created | 2026-05-22 |
| Last push | 2026-08-06 |
| Open issues | 23 |
| Topic | security |

> [!warning] Maintenance status, stated by the repo `[FACT]`
> "This repo is **not maintained** and is not accepting contributions." Read it as a
> **reference**, exactly as its name says.

## Why it matters

`[INFERENCE]` It is the only **first-party, complete, runnable** example in this vault of the
whole stack composed together: instruction file, skills, an autonomous loop, sandboxing, and
verification — from the vendor, in a repo you can clone. Everything else is either a blog post
describing a private system or a third-party reconstruction.

## Architecture `[FACT — root listing]`

```text
CLAUDE.md          the instruction file
.claude/           skills and configuration
harness/           the autonomous pipeline
dnr_harness/       detection & response
targets/           what it runs against
tests/  bin/  scripts/  docs/  static/
DEMO.md
```

## The loop `[FACT — README]`

> "**`harness/`**: the autonomous reference pipeline (**recon → find → verify → report →
> patch**), configured for finding C/C++ memory vulnerabilities using **Docker and ASAN**."

Five stages, and the two in the middle are the interesting ones:

`[INFERENCE]` **`verify` sits between `find` and `report`** — findings are validated before
anyone sees them. That is [[Generator Evaluator Separation]] as a pipeline stage, and it is
what makes an unattended security scanner tolerable: an unverified finding costs a human triage
cycle, so a scanner without a verify stage generates work rather than saving it. The same
lesson as this vault's own [[Research Integrity in Agent-Assisted Research|fabrication incident]].

**Docker and ASAN** are the ground truth. `[INFERENCE]` ASAN is a perfect judge within its
scope — a memory error either reproduces under the sanitiser or it does not, with no model
judgment involved. Choosing a domain with a deterministic oracle is what makes the autonomy
safe, and it is the same reason [[Autonomous Test Fixer]] is the right first loop to build.

## Skills `[FACT]`

`/quickstart`, `/threat-model`, `/vuln-scan`, `/triage`, `/patch`, `/customize`, plus
`/dnr-hunt` for detection and response. Open the repo in Claude Code and run `/quickstart`.

`[INFERENCE]` The set is a clean worked example of skills as **procedures**, not knowledge:
each is a repeatable pipeline stage that a human can also invoke individually. The pipeline and
the interactive skills are the same logic at two levels of autonomy.

## The honesty worth copying `[FACT]`

> "This harness is a **reference, not a product**. The general shape, prompts, and sandboxing
> are reusable, but the harness **will not work on every codebase out of the box**. Run
> `/customize` to port it to your language, detector, or vuln class."

`[INFERENCE]` Note that `/customize` exists at all: rather than claiming generality, they
shipped a skill whose job is porting. That is the correct response to
[[When Not to Build a Harness|the generality problem]] — the shape transfers, the configuration
does not, so automate the porting instead of pretending it is unnecessary.

## What to study

1. **`CLAUDE.md`** — a first-party instruction file, against [[Instruction File Design]].
2. **`harness/`** — the five-stage loop, especially where verification sits.
3. **`.claude/`** — first-party skill layout.
4. **The sandboxing** — Docker + ASAN as both isolation and oracle. See
   [[Sandboxing and Permissions]].
5. **`/customize`** — porting a harness as an automated procedure.

## Limitations

- Unmaintained; 23 open issues.
- Narrow domain: C/C++ memory safety. The *shape* generalises, the detectors do not.
- `NOASSERTION` licence — check `LICENSE` before reuse.
- `[FACT]` Anthropic points to a hosted product for a managed alternative; treat the repo as
  educational.

## Related

- [[Generator Evaluator Separation]] · [[Autonomous Test Fixer]] · [[Sandboxing and Permissions]]
- [[Claude Code as a Harness]] · [[Loop Types]] · [[Repository Index]]
