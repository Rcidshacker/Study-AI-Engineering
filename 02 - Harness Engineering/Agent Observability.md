---
title: Agent Observability
aliases:
  - Observability for agents
  - Making the runtime legible
tags:
  - harness-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Agent Observability

> [!abstract] One line
> Two distinct problems that share a word: making **the running system** legible to the agent,
> and making **the agent** legible to you. The first is a verification capability; the second is
> how you keep unattended work trustworthy.

---

## Direction 1 — the system, legible to the agent

### What OpenAI built `[FACT]`

The bottleneck as throughput rose was **human QA capacity**, so they made the application
itself readable by Codex:

- The app is **bootable per git worktree** — one instance per change.
- The **Chrome DevTools Protocol** is wired into the agent runtime, with skills for DOM
  snapshots, screenshots and navigation. This let Codex "reproduce bugs, validate fixes, and
  reason about UI behavior directly."
- Logs, metrics and traces come from a **local observability stack, ephemeral per worktree**.
  Agents query logs with **LogQL** and metrics with **PromQL**.

The payoff, stated as example prompts that became tractable:

> "ensure service startup completes in under 800ms"
> "no span in these four critical user journeys exceeds two seconds"

### The general principle `[INFERENCE]`

> **Any property you want the agent to maintain must be queryable by the agent.**

Otherwise it is a wish. Latency, error rates, log cleanliness, memory use — none of these can
be maintained by an agent that cannot measure them, no matter how firmly the instruction file
asks. This is [[The Verification Gap]] applied to runtime behaviour rather than correctness.

### The cheaper versions

You do not need a Grafana stack to start. `[INFERENCE]` Ordered by cost:

| Level | Gives the agent |
|---|---|
| A `logs/` file it can read | did it crash, and where |
| A `check.sh` printing structured results | pass/fail with detail |
| Browser automation | what the user actually sees — `[FACT]` Anthropic's fix for end-to-end blindness |
| Local metrics endpoint | timings and counters |
| Full per-worktree stack | trends, traces, SLOs |

`[FACT]` Claude Code ships bundled `/run` and `/verify` skills that "launch and drive your app"
and confirm a change "without falling back to tests or type checks" — the lowest-effort entry
to this direction.

---

## Direction 2 — the agent, legible to you

`[FACT]` `mini-swe-agent` saves the trajectory **in a `finally` block**, so it survives crashes.
Observability is structural, not optional.

`[FACT]` Claude Code documents an **agent view** for managing multiple agents, `SubagentStart`
and `SubagentStop` hook events, OpenTelemetry observability in the Agent SDK, and cost/usage
tracking.

`[FACT]` Ralph's `progress.txt` is the low-tech version and it works: what was implemented,
which files, what was learned.

### What to record `[INFERENCE]`

| Record | Answers |
|---|---|
| Full transcript | what did it actually do |
| Which exit status fired | success, budget, stuck, or error — see [[Stopping Conditions]] |
| Files touched | was scope respected |
| Check results per iteration | was it converging or oscillating |
| Cost and elapsed time | is this economic |

The most-skipped item is the **exit status**. A loop that stops for one reason and is read as
having stopped for another is the commonest silent failure in unattended automation.

---

## The asymmetry `[INFERENCE]`

Observability **scales worse than execution**. Five parallel agents are trivial to start and
genuinely hard to understand afterwards. That asymmetry is a real argument against reaching
for a graph before you need one — see [[Claude Code Graphs]] — and an argument for logging to
**files** rather than to a terminal you will not read.

---

## The reading habit that pays most `[INFERENCE]`

Read one **complete** unattended transcript, end to end, before trusting a loop. Not the
summary, not the diff — the whole thing. It is the only way to see the gap between what you
believe your controls constrain and what they actually constrain, and it is the single most
educational hour in [[Learning Roadmap|the roadmap]].

---

## Related

- [[The Verification Gap]] · [[Stopping Conditions]] · [[Worktree Isolation]] · [[Feedback Quality]]
- [[Claude Code Implementation Notes]] · [[Production Coding Agent]] · [[Loop Failure Modes]]
- [[Source - OpenAI Harness Engineering]]
