---
title: Claude Code Hooks
aliases:
  - Hooks
  - Lifecycle hooks
tags:
  - claude-code
  - harness-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Claude Code Hooks

> [!abstract] One line
> Hooks are the only way to make something **deterministically always happen** inside a
> non-deterministic loop. That makes them the highest-leverage outer-harness surface Claude
> Code exposes.

> [!info] Verification
> Read from the official hooks reference at `code.claude.com/docs/en/hooks.md` on
> **2026-09-04**. The reference is large (~317KB) and detailed; this note covers the parts
> that matter for harness and loop design, not the full schema. Go to the reference for exact
> configuration syntax.

---

## What a hook is `[FACT]`

> "Hooks are user-defined **shell commands, HTTP endpoints, MCP tool calls, LLM prompts, or
> subagents** that execute automatically at specific points in Claude Code's lifecycle."

And they are portable across surfaces: "Claude Code fires the same hook events wherever it
runs: sessions in the terminal, IDE extensions, the Desktop app, and Claude Code on the web."

`[INFERENCE]` That handler list is the key fact. A hook is not just "run a script." It spans
the whole of [[Guides and Sensors]]:

| Handler | Control type | Example |
|---|---|---|
| shell command | **computational** | run the type checker after an edit |
| MCP tool call | computational | call a service |
| HTTP endpoint | computational | notify a system |
| **LLM prompt** | **inferential** | "does this diff match the acceptance criteria?" |
| **subagent** | **inferential, isolated context** | a full independent review |

The last one is the important one: a hook that runs a subagent is
[[Generator Evaluator Separation]] fired **automatically**, rather than when you remember to
ask for a review.

---

## The three cadences `[FACT]`

> Events fall into three cadences:
> - **once per session**: `SessionStart`, `SessionEnd`
> - **once per turn**: `UserPromptSubmit`, `Stop`, `StopFailure`
> - **on every tool call** inside the agentic loop: `PreToolUse`, `PostToolUse`

`[INFERENCE]` The cadence is the design decision. Map it onto harness roles:

| Cadence | Harness role | Canonical use |
|---|---|---|
| per session | initialisation, state loading | run `init.sh`; inject the progress file and today's goal |
| per turn | the **loop boundary** | verification gate; decide whether the loop continues |
| per tool call | guardrails | block writes outside scope; run fast checks after edits |

---

## The event surface `[FACT — names from the reference]`

Far larger than most people use. Beyond the core six, the reference documents (among others):
`InstructionsLoaded`, `UserPromptExpansion`, `MessageDisplay`, `PermissionRequest`,
`PostToolUseFailure`, `PostToolBatch`, `PermissionDenied`, `Notification`, `SubagentStart`,
`SubagentStop`, `TaskCreated`, `TaskCompleted`, `TeammateIdle`, `ConfigChange`, `CwdChanged`,
`DirectoryAdded`, `FileChanged`, `WorktreeCreate`, `WorktreeRemove`, `PreCompact`,
`PostCompact`, `PreModelSwitch`, `PostModelSwitch`, `Elicitation`.

`[INFERENCE]` Three of these deserve more attention than they get:

- **`PostToolUseFailure`** — the natural place to turn a bare error into a teaching message.
  See [[Feedback Quality]].
- **`PreCompact` / `PostCompact`** — compaction is lossy
  ([[Source - Anthropic Effective Harnesses for Long-Running Agents]]: "compaction isn't
  sufficient"). These are the seam at which to write critical state to disk *before* it is
  summarised away. See [[External State]].
- **`SubagentStop`** — where a graph node's output can be validated before it is trusted.

---

## The four hooks worth building first

Ranked by return, `[INFERENCE]`:

### 1. `PostToolUse` on edits → run the fast checks

Type check, lint, focused tests. Return the failure output **with remediation text**. This
closes [[The Verification Gap]] at the moment the mistake is made, which is when correction is
cheapest. The reference has a worked "run tests after file changes" example, including running
it asynchronously.

### 2. `Stop` → the verification gate

The loop boundary. A script for deterministic checks, a prompt for model-evaluated ones. This
is how you get a maker-checker loop that fires every turn without typing anything. See
[[Claude Code Loops]] and [[Autonomous Test Fixer]].

`[FACT]` The reference has a section on checking **multiple conditions** before stopping.

### 3. `PreToolUse` on writes → scope enforcement

Block edits outside the directory the current task owns; block edits to `tests/` during a
test-fixing loop. The computational guide that makes the instruction in `CLAUDE.md` real. See
[[Wrong File Modification]] and [[Executable Rules Beat Written Rules]].

### 4. `SessionStart` → load the position

Run `init.sh`, print the progress file, print the next unfinished feature. Turns
[[Coding Agent Startup Flow]] from an instruction the agent might follow into something that
has already happened.

---

## Why hooks beat instructions

| | A rule in `CLAUDE.md` | A hook |
|---|---|---|
| Enforcement | advisory | deterministic |
| Context cost | every session | none until it fires |
| Can be silently ignored | yes | no |
| Can *do* something | no | yes |
| Versioned | yes | yes |

`[INFERENCE]` The general move: **every time you write a rule, ask whether a hook could
enforce it instead.** A rule the agent must remember competes for attention with everything
else in the file. A hook simply happens. This is [[Executable Rules Beat Written Rules]] with
a concrete mechanism attached.

---

## Safety `[FACT]`

The reference carries its own "Security considerations," "Disclaimer," "Workspace trust" and
"Security best practices" sections. `[INFERENCE]` The reason is obvious once stated: a hook is
arbitrary code that runs automatically, configured from files that may come from a repository
you did not write. Treat hook configuration as **executable content**, review it as you would
a CI pipeline, and be deliberate about hooks arriving via plugins or cloned repos.

---

## Related

- [[Claude Code as a Harness]] · [[Claude Code Loops]] · [[Guides and Sensors]] · [[Feedback Quality]]
- [[Generator Evaluator Separation]] · [[Executable Rules Beat Written Rules]] · [[The Verification Gap]]
- [[Claude Code MOC]]
