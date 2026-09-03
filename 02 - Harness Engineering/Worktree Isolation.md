---
title: Worktree Isolation
aliases:
  - Git worktrees for agents
  - Parallel isolation
tags:
  - harness-engineering
  - environment
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Worktree Isolation

> [!abstract] One line
> One agent, one working tree. It is the prerequisite for running agents in parallel at all,
> and the cheapest way to bound the damage of an unattended run.

---

## Why it is a precondition, not an optimisation

`[INFERENCE]` Two agents in one working tree will corrupt each other's work, and the failure is
nasty: not a crash, but a half-applied change that passes some checks. No amount of instruction
prevents it, because both agents are behaving correctly given what they can see.

`[FACT]` It is one of the six building blocks of a loop in Osmani's decomposition —
"**Worktrees: parallel isolation**" — and Claude Code documents "Run parallel sessions with
worktrees" as a first-class feature.

---

## What OpenAI actually did `[FACT]`

The strongest documented use goes well beyond source isolation:

- The **application is bootable per worktree**, so Codex "could launch and drive one instance
  per change."
- Logs, metrics and traces come from "a local observability stack that's **ephemeral for any
  given worktree**." The agent works on "a fully isolated version of that app — including its
  logs and metrics, which get torn down once that task is complete."

`[INFERENCE]` This is the important generalisation: **isolate the running system, not just the
files.** If two agents share a database, a port, or a log stream, they are not isolated no
matter how separate their checkouts are. That shared resource is where the confusing failures
come from.

---

## The trap that catches everyone `[FACT]`

Claude Code's `.worktreeinclude`, at the **project root**:

> "Worktrees are fresh checkouts, so untracked files like `.env` are missing by default. …
> Only files that match a pattern and are also gitignored get copied, so tracked files are
> never duplicated."

```text
# .worktreeinclude
.env
.env.local
config/secrets.local.json
```

`[INFERENCE]` Without this, every parallel session fails identically and immediately, in a way
that looks like an agent problem and is an environment problem. It is the first thing to check
when parallel runs break and single runs do not.

`[FACT]` It is git-only: if you configure a `WorktreeCreate` hook for a different VCS, the file
is not read and you copy files inside your hook script instead.

---

## What isolation buys, ranked

| Benefit | Note |
|---|---|
| **Parallelism becomes possible** | file collisions are physically prevented |
| **Blast radius is bounded** | an unattended loop cannot damage your working tree |
| **Comparison becomes cheap** | run the same task twice, diff the results |
| **Cleanup is trivial** | delete the worktree |
| **Verification becomes real** | per-worktree app + observability makes runtime properties checkable |

---

## Isolation levels

`[INFERENCE]` Match the level to the autonomy:

| Level | Isolates | Use when |
|---|---|---|
| Branch | history | interactive work you are watching |
| **Worktree** | **files + running app + observability** | **parallel agents, or unattended runs** |
| Container / VM | the whole OS | permissions are relaxed, or the code is untrusted |
| Cloud / self-hosted environment | everything, off your machine | long unattended runs, or shared team use |

`[FACT]` Claude Code documents branch work, worktrees, devcontainers, sandbox environments, and
cloud/self-hosted environments — the full ladder exists.

---

## The rule for unattended work

Anything running without you watching should have **all** of:

1. its own worktree or container,
2. its own branch,
3. a **committed** starting state,
4. a budget — see [[Stopping Conditions]],
5. permissions scoped to what the task needs — see [[Sandboxing and Permissions]].

`[FACT]` [[Ralph Loop]] runs `--dangerously-skip-permissions`, which makes items 1–3
non-negotiable rather than advisable. It reads `branchName` from its PRD for exactly this
reason.

---

## Related

- [[Sandboxing and Permissions]] · [[Harness Components]] · [[Agent Observability]]
- [[Claude Code Implementation Notes]] · [[Ralph Loop]] · [[Production Coding Agent]]
- [[Source - OpenAI Harness Engineering]]
