---
title: Claude Code Architecture
aliases:
  - How Claude Code works
tags:
  - claude-code
  - architecture
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Claude Code Architecture

> [!abstract] One line
> A model, five categories of tool, and a three-phase loop. The documentation calls the
> product itself "the agentic harness around Claude," which settles where it sits in
> [[The Unified Mental Model]].

> [!info] Verification
> Everything marked `[FACT]` was read from the official **"How Claude Code works"** page at
> `code.claude.com/docs/en/how-claude-code-works.md` on **2026-09-04**. The earlier version of
> this note was uncited.

---

## The three-phase loop `[FACT]`

> "When you give Claude a task, it works through three phases: **gather context**, **take
> action**, and **verify results**. These phases blend together."

The loop is adaptive rather than fixed:

> "A question about your codebase might only need context gathering. A bug fix cycles through
> all three phases repeatedly. A refactor might involve extensive verification. Claude decides
> what each step requires based on what it learned from the previous step, chaining dozens of
> actions together and course-correcting along the way."

`[INFERENCE]` Note that **verify** is a first-class phase in the vendor's own model, not an
afterthought — but the phase only does work if the environment gives it something to verify
*with*. The loop's shape is Anthropic's; the signal quality is yours. That is the whole
argument of [[The Verification Gap]].

---

## The vendor's own framing `[FACT]`

> "The agentic loop is powered by two components: **models** that reason and **tools** that
> act. **Claude Code serves as the agentic harness around Claude**: it provides the tools,
> context management, and execution environment that turn a language model into a capable
> coding agent."

This is `Agent = Model + Harness` in the product documentation, and it is the strongest
available evidence for placing the harness *beneath* the loop rather than above it. See
[[Lineage of the Word Harness]] and [[The Unified Mental Model]].

---

## Models `[FACT]`

Multiple models with different trade-offs, switchable with `/model` mid-session or
`claude --model <name>` at launch. The docs' guidance: "Sonnet handles most coding tasks well.
Opus provides stronger reasoning for complex architectural decisions."

And a clarification worth keeping: "When this guide says 'Claude chooses' or 'Claude decides,'
it's the model doing the reasoning."

`[INFERENCE]` Model choice matters most for the **inferential** half of your controls — an
LLM-as-judge, a reviewer subagent, `/goal`'s completion check. A weak judge cannot be fixed by
a strong harness. Computational controls are model-independent by construction. See
[[Guides and Sensors]] and [[Harness Beats Model Choice]].

---

## Tools — the five categories `[FACT]`

> "Tools are what make Claude Code agentic. Without tools, Claude can only respond with text."

| Category | What Claude can do |
|---|---|
| **File operations** | read, edit, create, rename, reorganise |
| **Search** | find files by pattern, search content with regex, explore codebases |
| **Execution** | run shell commands, start servers, run tests, use git |
| **Web** | search, fetch documentation, look up error messages |
| **Code intelligence** | language-server navigation and live diagnostics |

> "Each tool use returns information that feeds back into the loop, informing Claude's next
> decision."

`[INFERENCE]` That sentence is the architectural reason [[Feedback Quality]] matters so much.
**Tool output is not a log; it is the next turn's input.** A command that prints a bare
`exit 1` and a command that prints a bare failure plus the remediation cost the same to run
and differ enormously in what the next step does.

Note also that **code intelligence is a computational sensor you get nearly free** in a typed
language — live type errors, delivered continuously, without you building anything.

---

## Execution, sessions, and safety `[FACT]`

The page documents, each with its own section:

- **Execution environments and interfaces** — terminal, IDE, desktop, web
- **Sessions** — work across branches, resume or fork
- **The context window** — and its management
- **Checkpoints** — "undo changes with checkpoints"
- **Permissions** — "control what Claude can do"

`[INFERENCE]` Checkpoints and git are two different undo mechanisms at two different layers.
Checkpoints are session-scoped and product-managed; git commits are durable, reviewable, and
readable by the agent itself. For unattended loops, **commit** — see [[Agent State]].

---

## The human is inside the loop by default `[FACT]`

> "You're part of this loop too. You can interrupt at any point to steer Claude in a different
> direction, provide additional context, or ask it to try a different approach. Claude works
> autonomously but stays responsive to your input."

`[INFERENCE]` This is the default *and* the bottleneck: your attention is the system's
throughput limit. Every technique in [[Loop Engineering]] exists to move you out of that
position. The docs' own advice for working within it — "**delegate, don't dictate**" and "it's
a conversation" — is the right posture until you have the verification to leave the loop
safely.

---

## What you can and cannot change

| | Yours? |
|---|---|
| The three-phase loop, tool implementations, system prompt, compaction strategy | **no** — inner harness |
| Instructions, skills, MCP servers, hooks, permissions, subagents, tests, environment | **yes** — outer harness |

See [[Inner Harness vs Outer Harness]] and [[Claude Code as a Harness]].

---

## Related

- [[Claude Code]] · [[Claude Code as a Harness]] · [[Claude Code Loops]] · [[Claude Code Graphs]]
- [[Agent Loops]] · [[The Unified Mental Model]] · [[Claude Code MOC]]
