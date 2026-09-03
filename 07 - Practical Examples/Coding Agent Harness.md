---
title: Coding Agent Harness
aliases:
  - Coding Agent Harness Example
  - Reference harness layout
tags:
  - claude-code
  - harness-engineering
  - implementation
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Coding Agent Harness — a reference layout

> [!abstract] One line
> A complete, reproducible outer harness for a real project, built in four stages, where each
> stage is justified by a failure it prevents.

> [!info] Verification
> The directory layout and file placements below were read from the official
> **"Explore the .claude directory"** documentation on **2026-09-04**. An earlier version of
> this note placed `CLAUDE.md` inside `.claude/` as the primary location and is corrected
> here. `[FACT]` marks documented placement; the *content* of each file is my own
> `[INFERENCE]` synthesis of the practices in this vault.

---

## The verified layout `[FACT]`

```text
your-project/
├── CLAUDE.md                 # project instructions, read every session
├── .mcp.json                 # project-scoped MCP servers  (root, NOT .claude/)
├── .worktreeinclude          # gitignored files to copy into new worktrees (root)
├── .claude/
│   ├── settings.json         # permissions, hooks, configuration  (committed)
│   ├── settings.local.json   # your personal overrides           (gitignored)
│   ├── rules/                # topic-scoped instructions, optionally path-gated
│   │   ├── testing.md
│   │   └── api-design.md
│   ├── skills/               # reusable prompts, invoked by name or auto-triggered
│   │   └── security-review/
│   │       ├── SKILL.md      # entrypoint: trigger, invocability, instructions
│   │       └── checklist.md  # supporting file bundled with the skill
│   ├── agents/               # subagents with their own context window
│   │   └── code-reviewer.md
│   ├── workflows/            # dynamic workflow scripts orchestrating many subagents
│   ├── commands/             # single-file prompts invoked with /name (legacy — prefer skills)
│   └── output-styles/
├── docs/                     # the system of record — see below
├── feature_list.json         # definition of done
├── progress.md               # cross-session state
├── init.sh                   # bring the environment up
├── src/
└── tests/
```

Three placement facts people get wrong `[FACT]`:

1. **`CLAUDE.md` lives at the project root.** It also works at `.claude/CLAUDE.md` "if you
   prefer to keep the project root clean," but root is the documented default.
2. **`.mcp.json` and `.worktreeinclude` live at the root**, *not* inside `.claude/`.
3. **`commands/` and `skills/` are now the same mechanism.** The docs say so explicitly and
   recommend skills for new work: "same `/name` invocation, plus you can bundle supporting
   files."

---

## Stage 1 — Instructions (15 minutes)

**Failure it prevents:** wrong package manager, ignored conventions, reinvented helpers.

`[FACT]` The documented guidance: target **under 200 lines**; longer files "still load in full
but may reduce adherence." And: "If something only matters for specific tasks, move it to a
skill or a path-scoped rule so it loads only when needed."

That is [[Context Window as a Budget]] as official advice. Combined with OpenAI's
**map-not-manual** finding, the shape is:

```markdown
# Project conventions

## Commands
- Build: `npm run build`
- Test:  `npm test`
- Lint:  `npm run lint`
- Full verification: `npm run check`      # runs all three

## Stack
- TypeScript, strict mode. React 19, functional components only.

## Hard constraints
- Named exports, never default exports.
- Parse external data at the boundary. Never trust an unvalidated shape.
- Never edit files under `tests/` to make a test pass.

## Where things are
- Architecture and layering rules: docs/ARCHITECTURE.md
- Design decisions and rationale:  docs/design-docs/index.md
- Current plan:                    docs/exec-plans/active/
- Known debt:                      docs/exec-plans/tech-debt-tracker.md
```

`[INFERENCE]` The **Full verification** line is the single highest-return item on this page.
One command, named in the file the agent always reads. Everything downstream — hooks, loops,
`/goal` — can invoke it.

**Path-scoped rules** are how you keep this short: `.claude/rules/testing.md` carries test
conventions and loads only when test files are in play.

---

## Stage 2 — Feedback (30 minutes)

**Failure it prevents:** [[The Verification Gap]] and [[False Completion]].

### 2a. A single verification command

```bash
# package.json
"check": "tsc --noEmit && eslint src --max-warnings 0 && vitest run"
```

### 2b. A hook that runs it after edits

`.claude/settings.json` — a `PostToolUse` hook on file edits. `[FACT]` Hooks can be shell
commands, HTTP endpoints, MCP tool calls, LLM prompts, or subagents, and fire "on every tool
call inside the agentic loop." See [[Claude Code Hooks]] for the exact schema.

### 2c. Make the failure teach

`[FACT]` Both OpenAI and Thoughtworks independently arrived at this — custom check output
should carry remediation instructions. See [[Feedback Quality]].

```bash
# scripts/check-boundaries.sh
if grep -rn ": any" src/ --include=*.ts; then
  cat <<'MSG'
✗ Untyped boundary data found above.

  This project parses external data at the boundary rather than trusting it.
  Fix: define a zod schema in src/schemas/ and parse there.
  Pattern:  docs/design-docs/core-beliefs.md#boundary-parsing
  Example:  src/schemas/user.ts
MSG
  exit 1
fi
```

`[INFERENCE]` That heredoc is a prompt. It is delivered at the moment of the mistake, at the
line of the mistake, and costs nothing when the code is clean.

---

## Stage 3 — State (30 minutes)

**Failure it prevents:** lost continuity, re-solved problems, unbounded "done".

### `feature_list.json` `[FACT — structure from Anthropic]`

```json
[
  {
    "id": "auth-01",
    "category": "functional",
    "description": "A user can sign in with email and password and land on the dashboard",
    "steps": [
      "Navigate to /login",
      "Enter valid credentials",
      "Submit",
      "Verify redirect to /dashboard",
      "Verify the session cookie is set"
    ],
    "passes": false
  }
]
```

Rules, from [[Feature List as Harness Primitive]]: entries are **end-to-end and user-visible**;
everything starts `false`; the agent may change **only** `passes`; it is **JSON**, because
models overwrite markdown more readily.

### `progress.md`

```markdown
## Codebase Patterns
<!-- general, reusable facts. Read this section first. -->
- API routes return `{ data, error }`. Never throw across a route boundary.
- Migrations must be idempotent: always `IF NOT EXISTS`.

---
## 2026-09-04 — auth-01
- Implemented email/password sign-in.
- Files: src/routes/login.ts, src/schemas/credentials.ts
- Learnings: the session cookie needs `sameSite: lax` or the redirect drops it.
```

`[FACT]` The two-tier structure with consolidated patterns **at the top** is
[[Ralph Loop]]'s, and the placement is deliberate: a fresh agent reads from the top.
Append, never replace. See [[External State]].

### `init.sh`

`[FACT]` From Anthropic's design: the initializer writes a script that brings the environment
up, so no later session rediscovers it. Wire it to a `SessionStart` hook and it stops being
something the agent must remember.

---

## Stage 4 — Environment and isolation (20 minutes)

**Failure it prevents:** [[Wrong File Modification]], collisions between parallel agents,
unbounded blast radius on unattended runs.

- **Permissions** in `.claude/settings.json` — an allowlist, not `--dangerously-skip-permissions`.
- **A `PreToolUse` hook** blocking writes outside the current task's directory, and any write
  to `tests/` during a test-fixing loop. See [[Executable Rules Beat Written Rules]].
- **`.worktreeinclude`** `[FACT]` — worktrees are fresh checkouts, so untracked files like
  `.env` are missing by default. List them here or every parallel session fails identically.
- **Lockfiles and a pinned runtime version** so the environment is self-describing.

See [[Worktree Isolation]] and [[Sandboxing and Permissions]].

---

## What this buys, mapped to the five subsystems

| Subsystem | Artefact |
|---|---|
| Instructions | `CLAUDE.md` (map), `.claude/rules/`, `docs/` |
| Tools | built-ins, `.mcp.json`, scripts exposed via bash |
| Environment | `init.sh`, lockfiles, `.worktreeinclude`, permissions |
| State | `feature_list.json`, `progress.md`, git |
| Feedback | `npm run check`, `PostToolUse` hook, teaching error messages |

That is full coverage per [[Harness Components]], in roughly **ninety minutes**. Everything
beyond it — loops, subagent reviewers, graphs — builds on this and should be added only when a
failure demands it. See [[When Not to Build a Harness]].

---

## Stage 5 — only once stages 1–4 hold

| Add | When |
|---|---|
| A `Stop` hook running `npm run check` | you want verification without asking for it |
| `/goal` or an external loop | "done" is machine-checkable and you want to walk away |
| A reviewer subagent via hook | tests cannot express what you need checked |
| A dynamic workflow | you have genuinely independent parallel work, or must not trust a finding |
| Garbage-collection tasks | drift is visible and recurring |

See [[Claude Code Loops]], [[Claude Code Graphs]], [[Harness Debt and Garbage Collection]].

---

## Related

- [[Harness Components]] · [[Claude Code as a Harness]] · [[Production Coding Agent]]
- [[Feature List as Harness Primitive]] · [[Feedback Quality]] · [[External State]]
- [[Autonomous Test Fixer]] · [[Learning Roadmap]]
