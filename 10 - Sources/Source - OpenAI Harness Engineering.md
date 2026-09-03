---
title: Source - OpenAI Harness Engineering
aliases:
  - Harness engineering leveraging Codex in an agent-first world
  - Lopopolo 2026
tags:
  - source
  - primary-source
  - harness-engineering
  - openai
source-type: engineering-blog
author: Ryan Lopopolo (Member of Technical Staff, OpenAI)
publisher: OpenAI
published: 2026-02-11
url: https://openai.com/index/harness-engineering/
reliability: high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - OpenAI Harness Engineering

> [!info] Verification
> Fetched and read directly on 2026-09-04. HTTP 200. Title as published:
> *"Harness engineering: leveraging Codex in an agent-first world"*, by Ryan Lopopolo,
> dated **February 11, 2026**. This is the most-cited origin point for the term
> **harness engineering** in the coding-agent sense. See [[Lineage of the Word Harness]].

## What it is

An engineering report on a five-month internal OpenAI experiment: building and shipping a
real internal-beta product with **zero lines of manually-written code**. Every line —
application logic, tests, CI config, docs, observability, internal tooling — was written by
Codex. Humans steered; agents executed.

## The hard numbers `[FACT]`

All stated directly in the post:

| Claim | Value |
|---|---|
| First commit to empty repo | late August 2025 |
| Elapsed at time of writing | ~5 months |
| Codebase size | on the order of **1 million lines** |
| Pull requests opened and merged | roughly **1,500** |
| Team size | started at **3** engineers, grown to **7** |
| Throughput | **3.5 PRs per engineer per day** (rising as the team grew) |
| Speed vs. hand-writing | estimated **~1/10th the time** |
| Longest observed single agent run | **upwards of 6 hours** on one task |
| `AGENTS.md` size | roughly **100 lines** |

> [!warning] Read the scope limit
> The post explicitly says the end-to-end autonomy it describes "depends heavily on the
> specific structure and tooling of this repository and should not be assumed to
> generalize without similar investment—at least, not yet." Do not quote the throughput
> numbers as a general benchmark. See [[Harness Beats Model Choice]] for what does transfer.

## The claims that matter

### 1. The engineer's job is redefined `[FACT]`

The post states the team's job stopped being "write code" and became **"design environments,
specify intent, and build feedback loops."** This triple is the closest thing the field has
to a one-line job description for harness engineering. It maps onto three of the five
subsystems in [[Harness Components]].

When something failed, the fix was "almost never *try harder*." The human question was
always: **"what capability is missing, and how do we make it both legible and enforceable
for the agent?"** This is the core reflex of [[Fix the Class Not the Instance]].

### 2. The repository is the system of record `[FACT]`

> "From the agent's point of view, anything it can't access in-context while running
> effectively doesn't exist."

Knowledge in Google Docs, Slack threads, or people's heads is invisible. Only
repository-local, versioned artifacts count. See [[The Repository as System of Record]].

### 3. Give a map, not a manual `[FACT]`

The team tried the "one big `AGENTS.md`" approach and it failed in four named ways:

1. **Context is a scarce resource** — a giant file crowds out the task and the code.
2. **Too much guidance becomes non-guidance** — when everything is important, nothing is.
3. **It rots instantly** — a monolithic manual becomes "a graveyard of stale rules."
4. **It's hard to verify** — a single blob resists mechanical freshness/ownership checks.

The fix: `AGENTS.md` is a **table of contents** (~100 lines); a structured `docs/`
directory is the system of record. See [[Instruction File Design]] and
[[Context Window as a Budget]].

The published layout:

```text
AGENTS.md
ARCHITECTURE.md
docs/
├── design-docs/
│   ├── index.md
│   └── core-beliefs.md
├── exec-plans/
│   ├── active/
│   ├── completed/
│   └── tech-debt-tracker.md
├── generated/
│   └── db-schema.md
└── product-specs/
```

### 4. Enforce invariants, don't micromanage implementation `[FACT]`

The team requires Codex to parse data shapes at the boundary but does **not** prescribe the
library. Architecture is a rigid layered model — `Types → Config → Repo → Service →
Runtime → UI` — with cross-cutting concerns (auth, connectors, telemetry, feature flags)
entering only through an explicit `Providers` interface. Everything else is disallowed and
**enforced mechanically via custom linters and structural tests** (themselves
Codex-generated).

> "This is the kind of architecture you usually postpone until you have hundreds of
> engineers. With coding agents, it's an early prerequisite."

Crucially: because the lints are custom, the team **writes the error messages to inject
remediation instructions into agent context.** The linter is not just a gate — it is a
feedback channel. See [[Executable Rules Beat Written Rules]] and [[Feedback Quality]].

### 5. Legibility as the design objective `[FACT]`

The repo is optimised first for *Codex's* legibility, not human style preference:

- Favour "boring" technologies — composable, API-stable, well-represented in training data.
- Sometimes cheaper to have the agent **reimplement a subset** of a library than to work
  around opaque upstream behaviour. Named example: instead of a `p-limit`-style package,
  they built their own map-with-concurrency helper with 100% test coverage and native
  OpenTelemetry instrumentation.
- The output "does not always match human stylistic preferences, and that's okay."

### 6. Application legibility: make the running app readable by the agent `[FACT]`

As throughput rose, the bottleneck became **human QA capacity**. The response:

- The app is **bootable per git worktree**, so Codex drives one instance per change.
- The **Chrome DevTools Protocol** is wired into the agent runtime, with skills for DOM
  snapshots, screenshots, and navigation.
- Logs, metrics and traces are exposed through a **local observability stack that is
  ephemeral per worktree**. Agents query logs with LogQL and metrics with PromQL.

This is what makes prompts like *"ensure service startup completes in under 800ms"*
tractable. See [[Agent Observability]] and [[Worktree Isolation]].

### 7. Throughput changes the merge philosophy `[FACT]`

Minimal blocking merge gates. Short-lived PRs. Test flakes handled with a follow-up run
rather than indefinite blocking.

> "In a system where agent throughput far exceeds human attention, corrections are cheap,
> and waiting is expensive. This would be irresponsible in a low-throughput environment.
> Here, it's often the right tradeoff."

`[OPINION]` This is the most context-dependent recommendation in the post and the one most
often quoted out of context. It presupposes agent-driven review, mechanical invariants, and
continuous garbage collection. Without those, it is simply lowered standards.

### 8. Entropy and garbage collection `[FACT]`

Codex replicates existing patterns in the repo, including bad ones — so drift is inevitable.
The team initially spent **every Friday (20% of the week) cleaning up "AI slop."** That did
not scale. The replacement:

- Encode **"golden principles"** — opinionated, mechanical rules — into the repo.
- Run **background Codex tasks on a cadence** that scan for deviations, update quality
  grades, and open targeted refactoring PRs.
- Most such PRs are reviewable in under a minute and automerged.

> "This functions like garbage collection. Technical debt is like a high-interest loan."

This is a [[Loop Engineering]] pattern living inside a harness — see
[[Harness Debt and Garbage Collection]].

### 9. The autonomy ladder `[FACT]`

The eleven-step sequence Codex can drive from a single prompt: validate current state →
reproduce bug → record video of failure → implement fix → validate by driving the app →
record video of resolution → open PR → respond to feedback → detect and remediate build
failures → escalate to human only when judgment is required → merge.

See [[Levels of Agent Autonomy]].

## Where it points elsewhere `[FACT]`

The post explicitly names the **Ralph Wiggum Loop** as the shape of its PR-completion loop
(iterate until all agent reviewers are satisfied). See [[Ralph Loop]] and
[[GitHub - snarktank ralph]].

## How to use this source

- It is the **strongest available evidence** that harness investment, not model choice,
  produced the result — the model was not swapped mid-experiment.
- It is a **single-team, single-product report**, not a controlled study. Treat effect sizes
  as illustrative.
- The practices in §3, §4, §6 and §8 are directly reproducible in [[Claude Code]] today.
  See [[Production Coding Agent]].

## Related

- [[Harness Engineering]] · [[Harness Components]] · [[The Repository as System of Record]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]] — the earlier, complementary account
- [[Source - Learn Harness Engineering Course]] — teaches these ideas as a curriculum
- [[Sources MOC]]
