---
title: Production Coding Agent
aliases:
  - Scenario 8
  - Production harness
tags:
  - scenario
  - harness-engineering
  - production
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Scenario — Production Coding Agent

> [!abstract] The task
> A real software project, a real team, agents doing most of the writing. What has to be true
> for that to be responsible rather than reckless?

> [!note] Rewritten 2026-09-04
> The earlier version was uncited. This one is built from the only detailed public account of
> a team actually doing it: [[Source - OpenAI Harness Engineering]], plus the failure modes
> from [[Source - Anthropic Effective Harnesses for Long-Running Agents]].

---

## The one published data point `[FACT]`

Five months, one internal product, **zero lines of manually-written code**:

| | |
|---|---|
| Codebase | ~1,000,000 lines |
| PRs merged | ~1,500 |
| Team | 3 engineers, growing to 7 |
| Throughput | 3.5 PRs per engineer per day |
| Speed vs hand-writing | ~1/10th the time |
| Longest single agent run | 6+ hours |

> [!warning] Do not read this as a benchmark
> The post says plainly that this "depends heavily on the specific structure and tooling of
> this repository and **should not be assumed to generalize without similar investment**."
> Copy the practices; do not quote the throughput.

---

## The seven things that made it work

### 1. The repository is the system of record `[FACT]`

> "From the agent's point of view, anything it can't access in-context while running
> effectively doesn't exist."

Slack threads, Google Docs and people's heads are invisible. The architectural decision made
in a meeting must land in `docs/` or it did not happen. See
[[The Repository as System of Record]].

### 2. A map, not a manual `[FACT]`

`AGENTS.md` at ~100 lines as a table of contents; `docs/` as the real knowledge base, read on
demand. The one-big-file approach failed four documented ways: context crowding, guidance
dilution, instant rot, unverifiability. See [[Instruction File Design]].

### 3. Invariants enforced mechanically, implementation left free `[FACT]`

A rigid layered architecture — `Types → Config → Repo → Service → Runtime → UI`, with
cross-cutting concerns entering only through an explicit `Providers` interface — **enforced by
custom linters and structural tests**, themselves agent-generated.

> "This is the kind of architecture you usually postpone until you have hundreds of engineers.
> With coding agents, it's an early prerequisite: the constraints are what allows speed without
> decay."

And the boundary is deliberate: they require parsing at the boundary but do **not** prescribe
the library. "Enforce boundaries centrally, allow autonomy locally."

### 4. Failures teach `[FACT]`

> "Because the lints are custom, we write the error messages to inject remediation
> instructions into agent context."

Independently arrived at by Thoughtworks two months later. See [[Feedback Quality]].

### 5. The running application is legible `[FACT]`

- app **bootable per git worktree** — one instance per change
- **Chrome DevTools Protocol** in the agent runtime, with DOM/screenshot/navigation skills
- **per-worktree ephemeral observability**: logs via LogQL, metrics via PromQL

This is what makes *"ensure service startup completes in under 800ms"* a checkable requirement
rather than a wish. See [[Agent Observability]] and [[Worktree Isolation]].

### 6. Review is agent-to-agent, and merge philosophy changes `[FACT]`

Codex reviews its own changes locally, requests additional specific agent reviews locally and
in the cloud, and iterates "until all agent reviewers are satisfied" — a
[[Ralph Loop|Ralph Wiggum loop]], named as such in the post. Humans may review but are not
required to.

Merge gates are minimal, PRs short-lived, flakes handled with a re-run:

> "In a system where agent throughput far exceeds human attention, corrections are cheap, and
> waiting is expensive. **This would be irresponsible in a low-throughput environment.**"

`[OPINION]` This is the most-quoted-out-of-context sentence in the field. It presupposes
agent-driven review, mechanical invariants, and continuous cleanup. Without all three, it is
simply lowered standards.

### 7. Entropy is fought continuously `[FACT]`

Agents replicate existing patterns, including bad ones, so drift is guaranteed. The team spent
**every Friday — 20% of the week — cleaning up "AI slop"** before automating it: "golden
principles" encoded in the repo, plus background tasks on a cadence that scan for deviations,
update quality grades, and open targeted refactoring PRs, most automergeable in under a minute.

> "This functions like garbage collection. Technical debt is like a high-interest loan."

See [[Harness Debt and Garbage Collection]].

---

## Human approval, placed deliberately

`[INFERENCE]` Combining both primary sources, the humans in this system do four things and
only four:

1. **Prioritise** — decide what is worth building.
2. **Translate user feedback into acceptance criteria** — the feature list.
3. **Validate outcomes** — spot-check against reality.
4. **Repair the environment** — when the agent struggles, ask what capability is missing, and
   have the agent write the fix.

`[FACT]` The escalation rule in their autonomy ladder: "escalate to a human **only when
judgment is required**." Not when the task is hard — when it needs a decision the repository
cannot supply.

---

## The readiness checklist

Before letting agents write most of your code, all of these should be true:

| | Check |
|---|---|
| ☐ | One command verifies the project end to end |
| ☐ | Architectural invariants are enforced by a program, not a paragraph |
| ☐ | Check failures carry remediation text |
| ☐ | The app is bootable and inspectable per worktree |
| ☐ | Done is defined as a machine-checkable artefact |
| ☐ | Review is done by something that did not write the code |
| ☐ | Unattended work is isolated, budgeted, and committed before it starts |
| ☐ | Something scans for drift on a schedule |
| ☐ | The escalation path produces a written question, not a guess |

`[INFERENCE]` A "no" anywhere above is not a reason to avoid agents — it is the next thing to
build. Order them by [[When Not to Build a Harness|the investment ranking]]: feedback first.

---

## Related

- [[Coding Agent Harness]] · [[Harness Architecture]] · [[Harness Debt and Garbage Collection]]
- [[Autonomous Test Fixer]] · [[Levels of Agent Autonomy]] · [[Human In The Loop]]
- [[Source - OpenAI Harness Engineering]] · [[Scenarios MOC]]
