---
title: Claude Code as a Harness
aliases:
  - Claude Code harness surfaces
  - Claude Code Configuration
tags:
  - claude-code
  - harness-engineering
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Claude Code as a Harness

> [!abstract] One line
> Claude Code gives you an **inner harness** you cannot change and a large set of extension
> points that are your **outer harness**. This note maps every extension point onto the five
> subsystems, so you can audit coverage rather than collect features.

> [!info] Verification
> The feature list was read from the official documentation index at `code.claude.com/docs`
> on **2026-09-04**, including `features-overview.md` and the `.claude` directory page.
> `[FACT]` marks documented existence. It does **not** mean I ran every feature.
> Re-check before relying on any specific behaviour.

---

## What the documentation actually says the extensions do `[FACT]`

Quoted from `features-overview`:

- **CLAUDE.md** — "adds persistent context Claude sees every session"
- **Skills** — "add reusable knowledge and invocable workflows"
- **Code intelligence** — "connects Claude to a language server for symbol-level navigation
  and live type errors"
- **MCP** — "connects Claude to external services and tools"
- **Subagents** — "run their own loops in isolated context, returning summaries"
- **Dynamic workflows** — "run many subagents from a script Claude writes, returning one result"
- **Cross-session messaging** — "lets Claude pass a message from one of your sessions to another"
- **Hooks** — "run your script, HTTP request, MCP tool call, prompt, or subagent when Claude
  Code reaches a lifecycle event"
- **Plugins / marketplaces** — "package and distribute these features"

And the docs' own advice on order, which matches this vault's:

> "**New to Claude Code?** Start with CLAUDE.md for project conventions, then add other
> extensions as specific triggers come up."

`[INFERENCE]` "As specific triggers come up" is the whole discipline in five words. Add a
component when a failure demands it, never because a list exists. See
[[Fix the Class Not the Instance]] and [[When Not to Build a Harness]].

---

## The mapping: extension points → harness subsystems

Audit your project by filling in the right-hand column. An empty row is a real gap.

### 1. Instructions

| Surface | Notes |
|---|---|
| `CLAUDE.md` | project, user-level, and nested per-directory. Keep it a **map, not a manual** — see [[Instruction File Design]] |
| Auto memory | `[FACT]` documented as letting Claude "accumulate learnings automatically" — the promotion tier in [[Ralph Loop]], productised |
| Skills | markdown loaded **on demand**, so knowledge can be large without being always-on |
| Rules | `[FACT]` listed as part of the `.claude` directory; check the docs for current semantics |
| Output styles | shape how Claude responds, not what it knows |

> [!tip] The context-budget argument for skills
> `CLAUDE.md` costs context **every session**. A skill costs context **only when triggered**.
> That is the whole reason to move detail out of `CLAUDE.md` and into skills, and it is the
> same argument OpenAI makes for `AGENTS.md` as a table of contents. See
> [[Context Window as a Budget]].

### 2. Tools

| Surface | Notes |
|---|---|
| Built-in tools | file ops, search, execution, web access — "cover most coding tasks" per the docs |
| MCP servers | external services; the main way to add capability |
| Skills | can carry scripts, making a skill a tool as well as knowledge |
| Code intelligence (LSP) | symbol-level navigation and live type errors — a **computational sensor** you get almost free in typed languages |
| Bash | the general-purpose escape hatch — see [[Source - Anatomy of an Agent Harness]] |

### 3. Environment

| Surface | Notes |
|---|---|
| Worktrees | `[FACT]` "Run parallel sessions with worktrees" — the isolation primitive. See [[Worktree Isolation]] |
| Sandboxing | `[FACT]` a sandboxed Bash tool and selectable sandbox environments are documented |
| Devcontainers | documented for reproducible setup |
| Cloud / self-hosted environments | for running sessions off your machine |
| Hooks (SessionStart) | run your setup script — the `init.sh` slot |

### 4. State

| Surface | Notes |
|---|---|
| The repository | the real answer. See [[The Repository as System of Record]] |
| Sessions | `[FACT]` name, resume, branch, `--continue`, `--resume`, `--from-pr` |
| Checkpointing | `[FACT]` documented; rewind file changes |
| Auto memory | accumulated learnings across sessions |
| Your own files | `progress.md`, `feature_list.json` — still the most portable option |

`[INFERENCE]` Prefer **repo files** over product state for anything that must survive a tool
change, be reviewed by a human, or be diffed. Session resume is a convenience; a committed
progress file is an artefact.

### 5. Feedback

| Surface | Notes |
|---|---|
| Hooks | the key one — run a script, prompt, MCP call, or **subagent** at a lifecycle event |
| Subagents as reviewers | isolated context = an independent checker. See [[Generator Evaluator Separation]] |
| Code intelligence diagnostics | live type errors as the agent works |
| Code review / security scanning | `[FACT]` documented surfaces for review and vulnerability scanning |
| Your test and lint commands | still the highest-value item in the table |

---

## Hooks are the highest-leverage surface `[INFERENCE]`

Everything else adds *knowledge* or *capability*. Hooks add **deterministic control inside a
non-deterministic loop** — the only place you can make something *always* happen.

`[FACT]` A hook can run a script, an HTTP request, an MCP tool call, a prompt, or a subagent,
at a lifecycle event.

Read that list against [[Guides and Sensors]] and the design space opens up:

| Hook does | Control type | Example |
|---|---|---|
| runs a script | computational sensor | run the type checker after every edit |
| runs a prompt | inferential sensor | "does this diff match the acceptance criteria?" |
| runs a subagent | inferential sensor, isolated | full independent review |
| blocks at a lifecycle event | computational guide | refuse edits outside the feature directory |
| injects context at session start | inferential guide | load the progress file, today's goal |

`[INFERENCE]` The single highest-value hook for most projects is a **post-edit computational
sensor** — run the fast checks and hand the agent a failure message that says what to do. It
closes the [[The Verification Gap]] at the point of the mistake, which is when correction is
cheapest. See [[Feedback Quality]] and [[Claude Code Hooks]].

---

## Permissions as a harness component `[FACT]`

Documented: permission configuration, permission modes, a sandboxed Bash tool, and sandbox
environments; plus managed and server-managed settings for organisations.

`[INFERENCE]` Permissions are usually treated as a nuisance to be silenced. In harness terms
they are a **computational guide** — the cheapest possible control on
[[Wrong File Modification]]. The correct move is not `--dangerously-skip-permissions`; it is a
tight allowlist plus real isolation, so that when you *do* run unattended, the blast radius is
bounded. See [[Sandboxing and Permissions]].

---

## A minimum viable outer harness

If you do nothing else, do these five, in order:

1. **`CLAUDE.md`** — stack, commands, hard constraints, and pointers to docs. Under ~100 lines.
2. **Verification commands listed in it** — the single highest-return item.
3. **A post-edit hook** running the fast checks, with agent-readable failure messages.
4. **A permission allowlist** plus a worktree or branch for unattended work.
5. **A progress file and a feature list** committed to the repo.

That is the full five-subsystem coverage in [[Harness Components]], and it is perhaps two
hours of work. Everything past it is optimisation.

---

## Related

- [[Harness Components]] · [[Inner Harness vs Outer Harness]] · [[Guides and Sensors]]
- [[Claude Code Loops]] · [[Claude Code Graphs]] · [[Claude Code Hooks]] · [[Claude Code Skills]]
- [[Claude Code Architecture]] · [[Claude Code MOC]] · [[Production Coding Agent]]
