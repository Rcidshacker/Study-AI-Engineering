---
title: Sandboxing and Permissions
aliases:
  - Tool Permission Boundaries
  - Permissions
  - Wrong File Modification
tags:
  - harness-engineering
  - safety
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Sandboxing and Permissions

> [!abstract] One line
> Permissions are a **computational guide** — the cheapest control on the class of failure
> where the agent does the wrong thing to the wrong file. Treating them as friction to be
> switched off is the commonest safety mistake in agent work.

---

## The principle `[FACT]`

The course states both halves of the balance:

> "Do not disable shell for 'security reasons' — if the agent cannot even run `pip install`,
> how is it supposed to get anything done? But do not open everything either — follow the
> principle of **least privilege**."

`[FACT]` LangChain's rationale for sandboxes: "running agent-generated code locally is risky
and a single local environment doesn't scale to large agent workloads."

`[FACT]` Anthropic, on agents proper: their autonomy "means higher costs, and the potential for
**compounding errors**. We recommend extensive testing in sandboxed environments, along with
the appropriate guardrails."

---

## Scenario — the agent modifies the wrong files

**Why it happens** `[INFERENCE]`, in order of frequency:

1. **Scope was never expressed as a boundary.** "Work on the auth module" is a sentence. The
   filesystem is not partitioned, so every file is equally available.
2. **The agent is pattern-matching.** It found a similar symbol elsewhere and followed it. This
   is often *correct* behaviour — see [[Fix the Class Not the Instance]] — and occasionally
   catastrophic.
3. **The instruction competed with fifty others.** See [[Context Window as a Budget]].
4. **Nothing stopped it.** The only control was a sentence.

**The fix is layered.** Any single layer is defeatable; together they hold:

| Layer | Control type | Mechanism |
|---|---|---|
| Instruction | inferential guide | "this task owns `src/auth/`" — necessary, insufficient |
| **Permission rule** | **computational guide** | deny writes outside the task's directory |
| **`PreToolUse` hook** | **computational guide** | reject the edit, with a message saying what *is* allowed |
| Structural test | computational sensor | boundary violations fail the build |
| Worktree | environment | damage cannot escape the tree |
| Git | state | the change is revertible |

`[INFERENCE]` The hook is the interesting layer, because a rejection is also a *teaching*
moment: say what is permitted, not just that this was not. See [[Feedback Quality]].

---

## What Claude Code provides `[FACT — official docs, 2026-09-04]`

- **Permission configuration** and **permission modes**
- A **sandboxed Bash tool**
- Selectable **sandbox environments**
- **Devcontainers**; **cloud** and **self-hosted** environments
- **Managed** and **server-managed settings** for organisations
- **Checkpointing** to rewind file changes
- `PreToolUse`, `PermissionRequest` and `PermissionDenied` **hook events**

`[INFERENCE]` `PermissionDenied` is worth wiring up: repeated denials are a signal that either
your allowlist is wrong or the task is out of scope. Either way you want to know, and the
signal is free.

---

## The dangerous flag `[FACT]`

[[Ralph Loop]] runs `claude --dangerously-skip-permissions`; the Amp path uses
`--dangerously-allow-all`. Unattended loops generally need something like this, because a
permission prompt with nobody watching is a hang.

> [!warning] What must be true before you use it
> - a **container, VM, or dedicated worktree** — not your working tree
> - a **dedicated branch**
> - **everything committed** before starting
> - a **small iteration budget** on the first runs
> - you **read the log** afterwards
>
> `[INFERENCE]` The flag does not remove the need for containment; it **moves** it from the
> permission system to the environment. If you have not moved it, you have simply removed it.

---

## Sandboxing levels

`[INFERENCE]` Match to autonomy, not to paranoia:

| Level | Protects against | Cost |
|---|---|---|
| Permission allowlist | wrong tool, wrong path | prompts during interactive work |
| Worktree | damage to your tree, parallel collisions | ~none |
| Container / VM | anything the process can reach | setup |
| Remote environment | your machine entirely | latency, setup |

---

## The uncomfortable trade-off, stated plainly `[INFERENCE]`

Permissions create friction, and friction is why people disable them. The honest resolution is
not "tolerate the prompts" — it is **build a real allowlist once**, so the prompts stop for
routine work and still fire for the unusual. An allowlist you curated is a far better control
than a prompt you have been trained to approve reflexively.

---

## Related

- [[Worktree Isolation]] · [[Guides and Sensors]] · [[Harness Components]] · [[Claude Code Hooks]]
- [[Ralph Loop]] · [[Claude Code as a Harness]] · [[Stopping Conditions]]
- [[Source - Anthropic Building Effective Agents]] · [[Source - Anatomy of an Agent Harness]]
