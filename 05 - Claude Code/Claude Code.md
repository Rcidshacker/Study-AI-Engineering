---
title: Claude Code
aliases:
  - Claude Code overview
tags:
  - claude-code
  - hub
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Claude Code

> [!abstract] One line
> Anthropic's agentic coding tool: a model plus an **inner harness** you cannot change, plus a
> large set of extension points that are your **outer harness**.

> [!info] Verification and staleness
> Capability claims here were read from the official documentation index at
> `code.claude.com/docs` on **2026-09-04**. The docs ship a weekly *What's new* section, which
> tells you how fast this surface moves. **Re-read the docs before relying on any specific
> command or behaviour.** Notes in this vault mark `[FACT]` for documented existence, which is
> not the same as "I ran it."

---

## What it is `[FACT]`

Per the official overview: "an agentic coding tool that reads your codebase, edits files, runs
commands, and integrates with your development tools. Available in your terminal, IDE, desktop
app, and browser."

The core is [[Agent Loops|the agentic loop]] plus built-in tools for file operations, search,
execution and web access. The docs are explicit that "the built-in tools cover most coding
tasks" — everything else is an **extension layer**.

---

## The extension surface `[FACT]`

Quoted from the documentation:

| Extension | What it does |
|---|---|
| **CLAUDE.md** | "adds persistent context Claude sees every session" |
| **Skills** | "add reusable knowledge and invocable workflows" |
| **Code intelligence** | "connects Claude to a language server for symbol-level navigation and live type errors" |
| **MCP** | "connects Claude to external services and tools" |
| **Subagents** | "run their own loops in isolated context, returning summaries" |
| **Dynamic workflows** | "run many subagents from a script Claude writes, returning one result" |
| **Cross-session messaging** | "lets Claude pass a message from one of your sessions to another" |
| **Hooks** | "run your script, HTTP request, MCP tool call, prompt, or subagent when Claude Code reaches a lifecycle event" |
| **Plugins / marketplaces** | "package and distribute these features" |

Plus, from the docs index: sessions and checkpointing, permissions and permission modes, a
sandboxed Bash tool and sandbox environments, worktrees, agent teams and agent view, `/goal`,
`/loop`, scheduled tasks, channels, routines, headless mode, GitHub Actions and GitLab CI
integration, output styles, and the Agent SDK.

---

## How to read this vault's Claude Code notes

The three that matter, one per layer of [[The Unified Mental Model]]:

| Note | Layer | Answers |
|---|---|---|
| [[Claude Code as a Harness]] | harness | which extension point serves which of the five subsystems, and what a minimum viable setup is |
| [[Claude Code Loops]] | loop | `/goal` vs `/loop` vs Stop hooks vs external loops, and how to choose |
| [[Claude Code Graphs]] | graph | subagents, dynamic workflows, and when a graph is the wrong answer |

Supporting notes: [[Claude Code Architecture]], [[Claude Code Hooks]], [[Claude Code Skills]],
[[Claude Code MCP]], [[Claude Code Implementation Notes]].

---

## The one piece of advice the docs give, and this vault endorses `[FACT]`

> "**New to Claude Code?** Start with CLAUDE.md for project conventions, then add other
> extensions **as specific triggers come up**."

`[INFERENCE]` "As specific triggers come up" is the entire discipline compressed into five
words, and it is the opposite of how most people approach a feature list. Every extension you
add costs something — context, maintenance, or a new failure mode. Add it when a failure
demands it. See [[Fix the Class Not the Instance]] and [[When Not to Build a Harness]].

---

## Where Claude Code sits among the alternatives

`[FACT]` The other major terminal coding agents with public repositories, verified 2026-09-04:
`openai/codex` (121,228★, Rust), `anomalyco/opencode` (203,516★, TypeScript).

`[INFERENCE]` The relevant point for this vault is not which is better. It is that
**outer-harness work transfers between all of them.** A `feature_list.json`, an `init.sh`, a
test suite, custom linters with agent-readable error messages, an architecture-boundary
test — none of these care which agent reads them. The convergence on `AGENTS.md` as a
cross-vendor instruction file is the clearest sign of this. Invest in the portable layer. See
[[Inner Harness vs Outer Harness]].

---

## Related

- [[Claude Code as a Harness]] · [[Claude Code Loops]] · [[Claude Code Graphs]] · [[Claude Code MOC]]
- [[Harness Engineering]] · [[The Unified Mental Model]] · [[Production Coding Agent]]
