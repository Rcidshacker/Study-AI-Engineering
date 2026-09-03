---
title: Claude Code MOC
aliases:
  - Claude Code map
tags:
  - moc
  - claude-code
status: evergreen
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Claude Code MOC

> [!info] Verification standard for this section
> Every capability claim in these notes was read from the official documentation at
> `code.claude.com/docs` on **2026-09-04** — the docs index, `how-claude-code-works`,
> `features-overview`, `claude-directory`, `skills`, `hooks`, and `goal`. `[FACT]` means
> *documented*, not *I ran it*. The docs publish a **weekly** what's-new section, so treat this
> whole section as perishable and re-check before building on a command name.

---

## The notes

| Note | Covers |
|---|---|
| [[Claude Code]] | what it is, the extension surface, how to read this section |
| [[Claude Code Architecture]] | the three-phase loop, models, the five tool categories |
| [[Claude Code as a Harness]] | every extension point mapped to the five subsystems |
| [[Claude Code Loops]] | `/goal` vs `/loop` vs Stop hooks vs external loops |
| [[Claude Code Graphs]] | subagents, dynamic workflows, and when a graph is wrong |
| [[Claude Code Hooks]] | the highest-leverage surface: handlers, cadences, events |
| [[Claude Code Implementation Notes]] | skills, MCP, subagents, rules, memory — the working details |
| [[Coding Agent Harness]] | the reference `.claude/` layout, built in four stages |

---

## The vendor's own framing `[FACT]`

> "**Claude Code serves as the agentic harness around Claude**: it provides the tools, context
> management, and execution environment that turn a language model into a capable coding
> agent."

That sentence places the product squarely in [[The Unified Mental Model]] and is the best
available evidence for putting the substrate *beneath* the loop.

---

## The extension surface, in one table `[FACT — quoted from `features-overview`]`

| Extension | What it does |
|---|---|
| CLAUDE.md | "adds persistent context Claude sees every session" |
| Skills | "add reusable knowledge and invocable workflows" |
| Code intelligence | language-server navigation and live type errors |
| MCP | "connects Claude to external services and tools" |
| Subagents | "run their own loops in isolated context, returning summaries" |
| Dynamic workflows | "run many subagents from a script Claude writes, returning one result" |
| Cross-session messaging | pass a message from one of your sessions to another |
| Hooks | "run your script, HTTP request, MCP tool call, prompt, or subagent" at a lifecycle event |
| Plugins / marketplaces | package and distribute these |

Plus: sessions and checkpointing, permissions and permission modes, a sandboxed Bash tool and
sandbox environments, worktrees, agent teams and agent view, `/goal`, `/loop`, scheduled tasks,
channels, routines, headless mode, GitHub Actions and GitLab CI, output styles, and the
Agent SDK.

---

## Mapped to the layers

| Layer | Surfaces |
|---|---|
| **Harness** | `CLAUDE.md`, `.claude/rules/`, skills, MCP, permissions, sandboxing, worktrees, hooks, sessions, checkpointing |
| **Loop** | the built-in agentic loop, `/goal`, `/loop`, Stop hooks, scheduled tasks, channels, routines, headless, CI |
| **Graph** | subagents, dynamic workflows, agent teams, cross-session messaging, agent view |

---

## The advice the docs give, and this vault endorses `[FACT]`

> "**New to Claude Code?** Start with CLAUDE.md for project conventions, then add other
> extensions **as specific triggers come up**."

`[INFERENCE]` Five words carrying the whole discipline. Add a component when a failure demands
it — see [[Fix the Class Not the Instance]] and [[When Not to Build a Harness]].

---

## A minimum viable setup

1. `CLAUDE.md` under 200 lines: commands, stack, hard constraints, pointers.
2. **The verification command written down in it** — highest return of anything here.
3. A `PostToolUse` hook running the fast checks, with failure messages that teach.
4. A permission allowlist, plus a worktree for anything unattended.
5. A progress file and a `feature_list.json`, committed.

Roughly ninety minutes; full coverage of [[Harness Components]]. Details in
[[Coding Agent Harness]].

---

## Three details that are easy to get wrong `[FACT]`

- **`.mcp.json` and `.worktreeinclude` live at the project root**, not inside `.claude/`.
- **`CLAUDE.md` lives at the project root** (it also works at `.claude/CLAUDE.md`), target
  under 200 lines.
- **Commands and skills have merged.** `.claude/commands/deploy.md` and
  `.claude/skills/deploy/SKILL.md` both create `/deploy`; skills add bundled files and
  invocation control.

---

## Related

- [[AI Engineering MOC]] · [[Harness Loop Graph MOC]] · [[Learning Roadmap]]
- [[Production Coding Agent]] · [[Autonomous Test Fixer]] · [[Repository Index]]
