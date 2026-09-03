---
title: Harness Components
aliases:
  - Five subsystems
  - Harness Component Inventory
  - Harness subsystems
tags:
  - harness-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Harness Components

> [!abstract] One line
> Five subsystems — **instructions, tools, environment, state, feedback**. Miss one and the
> agent will feel awkward to use in a way you will struggle to name.

Three published inventories exist. They cut the same object along different axes, and the
differences are informative rather than contradictory.

---

## The three inventories

| | Cut by | Source |
|---|---|---|
| **Five subsystems** | *what the agent needs* | [[Source - Learn Harness Engineering Course]] |
| **Guides × Sensors, Computational × Inferential** | *direction and cost of control* | [[Source - Harness Engineering for Coding Agent Users]] |
| **Prompts / tools / infra / orchestration / middleware** | *what code you write* | [[Source - Anatomy of an Agent Harness]] |

Use the **five subsystems** to audit coverage, and [[Guides and Sensors]] to decide what to
build inside each.

---

## 1. Instructions

**What it is.** The standing rules: project overview and purpose, tech stack and versions,
first-run commands, non-negotiable constraints, and pointers to deeper docs.

**The design rule that matters** `[FACT]` — from [[Source - OpenAI Harness Engineering]]:
**give a map, not a manual.** `AGENTS.md`/`CLAUDE.md` is a **table of contents** of roughly
**100 lines**; the real knowledge lives in a structured `docs/` tree read on demand.

The four documented failure modes of the one-big-file approach: context crowding, "too much
guidance becomes non-guidance," instant rot, and unverifiability.

**Claude Code surface.** `CLAUDE.md` (project, user, and nested per-directory), plus skills
whose bodies load only when triggered.

See [[Instruction File Design]].

---

## 2. Tools

**What it is.** What the agent can actually *do*. Reading, writing, running commands, calling
services, driving a browser.

**The design rules.**

- `[FACT]` A general-purpose tool beats a large catalogue of specific ones. A harness "can
  only execute the tools it has logic for" — bash breaks that ceiling by letting the model
  write its own tools on the fly ([[Source - Anatomy of an Agent Harness]]).
- `[PRACTICE]` Do not disable the shell for vague "security" reasons — an agent that cannot
  install a dependency cannot finish work. Apply **least privilege**, not *no* privilege.
- `[INFERENCE]` Prefer interfaces the model has already seen millions of times — files,
  bash, git, JSON — over bespoke APIs it must be taught in-context. See
  [[The Filesystem as Harness Primitive]].

**Claude Code surface.** Built-in tools, permission rules in settings, MCP servers, skills.

See [[Tool Design for Agents]] and [[Sandboxing and Permissions]].

---

## 3. Environment

**What it is.** The runtime the agent acts in: dependencies, services, language versions,
isolation.

**The design rules.**

- Make environment state **self-describing** — lockfiles, `.nvmrc`/`.python-version`, Docker
  or devcontainers.
- `[FACT]` **Worktree isolation.** [[Source - OpenAI Harness Engineering]] makes the app
  bootable **per git worktree**, so each change gets its own instance. This is also the
  prerequisite for running agents in parallel at all. See [[Worktree Isolation]].
- `[FACT]` **An `init.sh`.** [[Source - Anthropic Effective Harnesses for Long-Running Agents]]
  has the initializer agent write a script that brings the environment up, so no later
  session has to rediscover how. See [[Initialization as a Phase]].

**Claude Code surface.** The working directory, hooks that run setup, MCP servers exposing
services, devcontainer config.

---

## 4. State

**What it is.** Everything that survives the end of a context window.

**Why it exists at all** `[FACT]`: the model is stateless. Each session "begins with no memory
of what came before" — engineers working in shifts, none remembering the previous shift
([[Source - Anthropic Effective Harnesses for Long-Running Agents]]).

**The design rules.**

- **The repository is the system of record.** Anything the agent cannot access in-context
  effectively does not exist. See [[The Repository as System of Record]].
- **A progress file** — what is done, in progress, blocked. Written before a session ends,
  read when the next begins.
- **Git history as memory** — descriptive commits let the agent reconstruct what happened
  *and* roll back to a working state.
- `[FACT]` **A feature list in JSON.** Anthropic found models are "less likely to
  inappropriately change or overwrite JSON files compared to Markdown files." The status
  field is the only thing agents may edit. See [[Feature List as Harness Primitive]].

**Claude Code surface.** Files in the repo, git, `CLAUDE.md` updates, session resume.

See [[Agent State]] and [[External State]].

---

## 5. Feedback

**What it is.** How the agent finds out whether what it did actually worked.

`[FACT]` This subsystem "usually has the lowest investment and highest return"
([[Source - Learn Harness Engineering Course]]). If you do one thing, do this one.

**The design rules.**

- **List verification commands explicitly** in the instruction file:

  ```text
  Verification commands:
  - Tests:      pytest tests/ -x
  - Type check: mypy src/ --strict
  - Lint:       ruff check src/
  - Full:       make check
  ```

- `[FACT]` **Write feedback for LLM consumption.** Custom linter messages should embed the
  remediation instruction. OpenAI: "we write the error messages to inject remediation
  instructions into agent context." Böckeler: "a positive kind of prompt injection."
  Two independent arrivals — see [[Feedback Quality]].
- `[FACT]` **End-to-end, not just unit.** Anthropic's agents ran unit tests and curl commands
  and still "failed to recognize that the feature didn't work end-to-end." Browser automation
  fixed it. See [[The Verification Gap]].
- `[FACT]` **Separate the maker from the checker.** Agents "confidently praise their own
  work." See [[Generator Evaluator Separation]].

**Claude Code surface.** Test commands, hooks that run checks after edits, subagents used as
reviewers, MCP browser tools.

---

## Auditing your own harness

`[PRACTICE]` Score each subsystem 1–5. Improve the lowest. Then use
**controlled-variable ablation** to find what is actually earning its keep: hold the model
fixed, remove one subsystem at a time, measure the drop.

The stated limit, which matters: ablation tells you which component is *most valuable right
now*, **not where the bottleneck is**. For that you need failure logs and root-cause
attribution. A component with near-zero ablation impact may be redundant, badly designed, or
simply not exercised by the task you tested. See [[Harness Ablation Testing]].

---

## Coverage check

| Subsystem | Symptom when missing |
|---|---|
| Instructions | picks wrong package manager, ignores conventions, reinvents existing helpers |
| Tools | stops and asks you to run things; cannot install or inspect |
| Environment | "works on my machine"; cannot reproduce; parallel runs collide |
| State | re-solves solved problems; loses the thread between sessions |
| Feedback | ships broken code confidently; declares victory early ([[False Completion]]) |

---

## Related

- [[Harness Engineering]] · [[Harness Architecture]] · [[Guides and Sensors]]
- [[The Verification Gap]] · [[Agent State]] · [[The Unified Mental Model]]
- [[Source - Learn Harness Engineering Course]] · [[Source - OpenAI Harness Engineering]]
