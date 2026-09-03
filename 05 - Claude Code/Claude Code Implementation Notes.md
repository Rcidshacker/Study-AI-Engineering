---
title: Claude Code Implementation Notes
aliases:
  - Claude Code cookbook
  - Claude Code Skills
  - Claude Code MCP
  - Claude Code Agents
tags:
  - claude-code
  - implementation
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Claude Code Implementation Notes

> [!abstract] One line
> Practical notes on the extension points, with the details that are easy to get wrong.
> Concepts live elsewhere; this is the working reference.

> [!info] Verification
> Read from the official documentation on **2026-09-04**: the docs index, `features-overview`,
> `claude-directory`, `how-claude-code-works`, `skills`, `hooks`, and `goal`. The earlier
> version of this note was uncited. **Product surfaces move weekly** — the docs carry a
> *What's new* section by calendar week. Re-check before relying on a specific command.

---

## Skills

### The facts that change how you use them `[FACT]`

- **Commands and skills have merged.** "A file at `.claude/commands/deploy.md` and a skill at
  `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way." Existing
  `commands/` files keep working. Skills add a directory for supporting files, frontmatter for
  invocation control, and automatic loading when relevant.
- **Skills follow the [Agent Skills](https://agentskills.io) open standard**, which works
  across multiple AI tools. Claude Code extends it with invocation control, subagent
  execution, and dynamic context injection. `[INFERENCE]` This makes skills **portable outer
  harness** — see [[Inner Harness vs Outer Harness]].
- **Where a skill lives decides its scope:**

  | Scope | Path |
  |---|---|
  | Personal, all projects | `~/.claude/skills/<name>/SKILL.md` |
  | Project only | `.claude/skills/<name>/SKILL.md` |
  | Plugin | `<plugin>/skills/<name>/SKILL.md` |
  | Enterprise | via managed settings |

### Bundled skills worth knowing `[FACT]`

Claude Code ships skills including `/doctor`, `/code-review`, `/batch`, `/debug`, `/loop`,
`/claude-api`, `/run`, `/verify`, `/run-skill-generator`.

Three of these are a ready-made answer to [[The Verification Gap]]:

| Skill | Purpose (quoted) |
|---|---|
| `/run` | "Launch and drive your app to see a change working" |
| `/verify` | "Build and run your app to confirm a code change does what it should, **without falling back to tests or type checks**" |
| `/run-skill-generator` | "Teach `/run` and `/verify` how to build and launch your project" |

`[FACT]` `/run` and `/verify` infer the launch from project type, README, `package.json` or
`Makefile` — and "that inference gets unreliable for projects that need anything beyond a
standard launch: a database, an env file, a graphical session, a multi-step build."
`/run-skill-generator` records the working recipe as a committed per-project skill at
`.claude/skills/run-<name>/`, "so any other agent in the repo follows the recorded recipe
instead of rediscovering it."

`[INFERENCE]` That is [[Fix the Class Not the Instance]] shipped as a feature: the discovery
happens once and becomes a durable artefact. Run it early on any project with a non-trivial
launch.

`[FACT]` `/verify` is one of the skills that runs **only when you invoke it**, "which keeps you
in control of when these longer-running checks spend time and tokens."

### The context argument for skills `[INFERENCE]`

`CLAUDE.md` costs context every session. A skill costs context only when triggered. So the
rule from the official memory guidance — "if something only matters for specific tasks, move
it to a skill or a path-scoped rule" — is a budget decision, not a tidiness one. See
[[Context Window as a Budget]].

---

## Hooks

Full treatment in [[Claude Code Hooks]]. The three facts that matter most:

1. A handler can be a **shell command, HTTP endpoint, MCP tool call, LLM prompt, or subagent**.
2. Events come in **three cadences**: once per session, once per turn, and on every tool call.
3. The same events fire in terminal, IDE, desktop and web.

`[INFERENCE]` "Subagent as a hook handler" is the sleeper feature: automatic
[[Generator Evaluator Separation]] without asking for a review.

---

## Subagents

`[FACT]` They "run their own loops in isolated context, returning summaries." Defined in
`.claude/agents/<name>.md`; a documented example is `code-reviewer.md`. There is also
`.claude/agent-memory/`, where "the subagent writes and maintains this file automatically."

`[INFERENCE]` The isolation cuts both ways. Use a subagent when **input is small, intermediate
work is large, output is small** — research, audit, review, search. Avoid one when the task
needs rich shared context, because information is lost at both boundaries. When two agents must
share a lot, share a **file**, not a message. See [[Claude Code Graphs]].

---

## MCP

`[FACT]` MCP "connects Claude to external services and tools." Configuration details that are
easy to get wrong:

- **`.mcp.json` lives at the project root, not inside `.claude/`.** It holds project-scoped
  servers the whole team shares.
- Personal servers go in `~/.claude.json` (`claude mcp add --scope user`).
- **Use environment variable references for secrets**: `"NOTION_TOKEN": "${NOTION_TOKEN}"`, so
  the token is read from your shell and never lands in the file.
- `[FACT]` "Tool schemas are deferred by default and load on demand via tool search" —
  a context-budget mechanism for large tool sets.

`[INFERENCE]` MCP is a **tools-subsystem** answer. It grants capability; it does not grant
verification. A new MCP server rarely fixes a reliability problem, and it adds tool
descriptions to every session's budget. Add one when a task genuinely could not be done.

---

## Permissions and isolation

`[FACT]` Documented: permission configuration, permission modes, a sandboxed Bash tool,
selectable sandbox environments, worktrees, and `.worktreeinclude` at the project root, which
lists gitignored files to copy into each new worktree because "worktrees are fresh checkouts,
so untracked files like `.env` are missing by default."

`[INFERENCE]` Miss `.worktreeinclude` and every parallel session fails identically, in a way
that looks like an agent problem and is an environment problem. See [[Worktree Isolation]].

---

## Rules

`[FACT]` `.claude/rules/` holds "topic-scoped instructions, optionally gated by file paths" —
documented examples `testing.md` (scoped to test files) and `api-design.md` (scoped to backend
code).

`[INFERENCE]` This is the cleanest available fix for a bloated instruction file: conventions
load only when the relevant files are in play. Move test conventions, API conventions and
frontend conventions out of `CLAUDE.md` and into path-scoped rules first.

---

## Memory

`[FACT]` `CLAUDE.md` at the project root (also works at `.claude/CLAUDE.md`), target **under
200 lines** — "longer files still load in full but may reduce adherence." Personal preferences
at `~/.claude/CLAUDE.md`. Auto memory at `~/.claude/MEMORY.md`, which "Claude writes and
maintains automatically," with topic notes split out "when MEMORY.md gets long." Edit with
`/memory`.

`[INFERENCE]` Auto memory is Ralph's promotion tier, productised. Treat it as convenience, and
keep anything a **teammate or a different agent** needs in the repo. See
[[The Repository as System of Record]].

---

## The order to add things

1. `CLAUDE.md` with commands and constraints
2. A single verification command, named in it
3. A `PostToolUse` hook running it, with teaching error messages
4. Permissions and a worktree for anything unattended
5. Path-scoped rules to keep `CLAUDE.md` short
6. Skills for repeatable procedures; `/run-skill-generator` if launching is non-trivial
7. Subagents when context isolation genuinely helps
8. MCP when a task is otherwise impossible

`[FACT]` The docs' own version: start with `CLAUDE.md`, then add extensions "as specific
triggers come up."

---

## Related

- [[Claude Code as a Harness]] · [[Claude Code Hooks]] · [[Claude Code Loops]] · [[Claude Code Graphs]]
- [[Coding Agent Harness]] · [[Context Window as a Budget]] · [[Claude Code MOC]]
