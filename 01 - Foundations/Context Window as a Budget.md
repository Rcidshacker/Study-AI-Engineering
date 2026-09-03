---
title: Context Window as a Budget
aliases:
  - Context is scarce
  - Context budget
tags:
  - context-engineering
  - harness-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Context Window as a Budget

> [!abstract] One line
> Context is a **zero-sum, per-turn budget**. Everything you load displaces something else,
> so the question is never "is this useful?" but "is this more useful than what it crowds out?"

---

## The claim `[FACT]`

[[Source - OpenAI Harness Engineering]], on why one big instruction file failed:

> "**Context is a scarce resource.** A giant instruction file crowds out the task, the code,
> and the relevant docs—so the agent either misses key constraints or starts optimizing for
> the wrong ones."

And the second-order effect, which is worse than the first:

> "**Too much guidance becomes non-guidance.** When everything is 'important,' nothing is.
> Agents end up pattern-matching locally instead of navigating intentionally."

`[INFERENCE]` That second point is the one people miss. Adding a rule does not just cost
tokens — it **dilutes every other rule**. A 50-line instruction file where every line matters
outperforms a 500-line file containing the same 50 lines.

---

## The architectural response `[FACT]`

[[Source - Wikipedia Agent Harness]] states the general principle:

> "Rather than repeatedly re-reading an ever-growing transcript inside the context window, a
> harness can offload record-keeping into a **structured software environment** that manages
> the agent's state."

This is why [[External State]] exists, why the filesystem is called the most foundational
harness primitive, and why [[Ralph Loop]] runs a *fresh* context every iteration rather than
accumulating one.

---

## The three moves

### 1. Map, not manual

`[FACT]` `AGENTS.md` at ~100 lines as a **table of contents**, with a structured `docs/`
directory as the system of record, read on demand. See [[Instruction File Design]].

### 2. On-demand over always-on

`[INFERENCE]` The costs differ by an order of magnitude in practice:

| Mechanism | Cost | When |
|---|---|---|
| `CLAUDE.md` | **every session**, whether relevant or not | invariants that apply to all work |
| Skill | only when triggered | deep procedural knowledge |
| File read | only when the agent chooses | reference material |
| Subagent | isolated — parent pays only for the **summary** | large exploration with a small answer |

This is the real argument for skills over a longer `CLAUDE.md`, and for subagents over
in-context research. Not "modularity" — **budget**.

### 3. Offload to disk, re-read the index

Progress files, status JSON and git history let a fresh context reconstruct position in three
cheap reads rather than carrying everything forward. And **ordering is interface**: `[FACT]`
Ralph puts consolidated patterns at the **top** of the progress file, because a fresh agent
reads from the top and may not reach the bottom.

---

## What each thing actually costs

`[INFERENCE]` A rough intuition, not measured figures:

| Item | Relative cost | Recurs? |
|---|---|---|
| A line in `CLAUDE.md` | small | **every session** — multiply by session count |
| A skill body | medium | only when triggered |
| Reading a large file | large | once per read, and it stays for the turn |
| A tool result | varies wildly | stays for the turn |
| Compaction | expensive, and **lossy** | when the window fills |

`[INFERENCE]` The item people under-price is the tool result. A command that prints 800 lines
of output costs far more than the instruction that invoked it — which is a good argument for
scripts that print *summaries plus remediation* rather than raw dumps. See
[[Feedback Quality]].

---

## The compaction caveat `[FACT]`

> "Compaction isn't sufficient."
> — [[Source - Anthropic Effective Harnesses for Long-Running Agents]]

Compaction "doesn't always pass perfectly clear instructions to the next agent." It is a
safety net for overflow, not a memory strategy. `[INFERENCE]` If your design depends on
compaction preserving something specific, that something belongs in a file.

---

## The practical audit

Open your `CLAUDE.md` and, for each line, ask:

1. Does this apply to **most** sessions? If not, it belongs in a skill or a doc.
2. Could a **check** enforce this instead of a sentence? If yes, move it —
   [[Executable Rules Beat Written Rules]].
3. Is it still true? Stale rules cost the same as live ones and mislead as well.

`[INFERENCE]` Most instruction files shrink by half under this audit and get *more* effective,
because the surviving lines stop competing with filler.

---

## Related

- [[Context Engineering]] · [[Instruction File Design]] · [[External State]] · [[Agent State]]
- [[Claude Code as a Harness]] · [[Ralph Loop]] · [[The Unified Mental Model]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Wikipedia Agent Harness]]
