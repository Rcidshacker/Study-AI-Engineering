---
title: Context Engineering
aliases:
  - Context window management
tags:
  - context-engineering
  - ai-engineering
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Context Engineering

> [!abstract] One line
> Deciding what the model sees on this turn. It is the layer between prompting and the
> environment, and it is the mechanism by which everything else reaches the model.

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose. This version is grounded in the OpenAI and
> Thoughtworks reports and the official Claude Code documentation.

---

## Definition and position

**Context engineering governs what information the model has when it decides.** Prompt
engineering shapes *how you phrase* this turn; context engineering shapes *what is in scope
for* this turn.

`[FACT]` Its position relative to a harness is **genuinely disputed**, and this is the one
place in the literature where sources contradict each other:

| Position | Source |
|---|---|
| The harness **contains** context engineering as a part | [[Source - Wikipedia Agent Harness]] |
| A coding-agent harness **is a form of** context engineering | [[Source - Harness Engineering for Coding Agent Users]] |

Böckeler states it plainly: "Context engineering provides us with the means to make guides and
sensors available to the agent. Engineering a user harness for a coding agent is a specific
form of context engineering."

`[INFERENCE]` Both hold along different axes. Böckeler is right *mechanically* — the way a
control reaches the agent is by entering its context. Wikipedia is right *architecturally* —
a sandbox, a permission boundary and a test runner are not context. The resolution this vault
uses: **context engineering is the harness's delivery mechanism; the harness is larger than
what it delivers.** Documented in [[The Unified Mental Model]].

---

## The governing constraint `[FACT]`

> "**Context is a scarce resource.** A giant instruction file crowds out the task, the code,
> and the relevant docs — so the agent either misses key constraints or starts optimizing for
> the wrong ones."

And the second-order effect, which is worse:

> "**Too much guidance becomes non-guidance.** When everything is 'important,' nothing is."

`[INFERENCE]` Adding context is not free even when the context is correct, because it dilutes
everything else. This is why context engineering is a *subtractive* discipline more often than
an additive one. See [[Context Window as a Budget]].

---

## The four moves

### 1. Map, not manual

`[FACT]` A ~100-line instruction file as a table of contents, with a structured `docs/` tree
read on demand. Claude Code's own docs give a compatible target: **under 200 lines**, and
"if something only matters for specific tasks, move it to a skill or a path-scoped rule so it
loads only when needed." See [[Instruction File Design]].

### 2. On-demand over always-on

| Mechanism | Paid |
|---|---|
| always-on instructions | every session |
| a skill | only when triggered |
| a file read | only when chosen |
| a subagent | parent pays only for the returned summary |

`[INFERENCE]` This is the real argument for skills and subagents. Not modularity — budget.

### 3. Offload to disk

`[FACT]` "Rather than repeatedly re-reading an ever-growing transcript inside the context
window, a harness can offload record-keeping into a structured software environment." See
[[External State]].

### 4. Make the environment legible

`[FACT]` The strongest form: OpenAI made the running application itself readable — DOM
snapshots, screenshots, and a per-worktree observability stack the agent queries with LogQL
and PromQL. Context is not only documents; **it is anything the agent can inspect at decision
time.**

---

## What counts as context

Broader than most people assume, and every item competes with every other:

- the system prompt and instruction files
- conversation history, including compacted summaries
- file contents the agent read
- **tool results** — often the largest single consumer, and the least controlled
- tool and skill *descriptions*, which are loaded before use
- error messages and check output
- what the environment makes queryable at all

`[INFERENCE]` Tool results are the underpriced item. A command that prints 800 lines costs far
more than the instruction that invoked it — which is a concrete argument for scripts that
print a summary plus remediation rather than a raw dump. See [[Feedback Quality]].

---

## The relationship to prompt engineering

`[FACT]` [[Source - Wikipedia Agent Harness]] frames prompt engineering as optimising "a
single interaction" and context engineering as governing "what information the model sees at a
given moment."

`[INFERENCE]` The practical distinction: prompt engineering is what you do when the model
*has* the information and misuses it. Context engineering is what you do when it never had it.
Diagnosing which one you face is the first branch in
[[The Unified Mental Model|the debugging order]]. See
[[Prompt Engineering vs Context Engineering]].

---

## Related

- [[Context Window as a Budget]] · [[Instruction File Design]] · [[External State]]
- [[The Unified Mental Model]] · [[Prompt Engineering vs Context Engineering]] · [[Harness Components]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Harness Engineering for Coding Agent Users]]
