---
title: Prompt Engineering vs Context Engineering
aliases:
  - Prompt Engineering
  - The naming stack
tags:
  - comparison
  - prompt-engineering
  - context-engineering
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Prompt Engineering vs Context Engineering

> [!abstract] One line
> Prompt engineering shapes **how you say it**. Context engineering shapes **what is in scope
> when the model decides**. Neither was replaced by the other; each new layer stacked on top.

> [!note] Rewritten 2026-09-04
> The earlier version treated these as successive fashions. They are cumulative.

---

## The distinction `[FACT]`

[[Source - Wikipedia Agent Harness]] draws it cleanly:

- **Prompt engineering** "optimises a single interaction."
- **Context engineering** "governs what information the model sees at a given moment."
- The harness "designs the whole operational environment."

---

## Diagnostic use — the reason the distinction earns its keep

`[INFERENCE]` Ask which failure you have, in this order:

| Question | If yes | Layer |
|---|---|---|
| Did it have the information and misuse it? | rephrase, add an example, constrain the format | **prompt** |
| Did it never have the information? | put the file, doc, or fact in scope | **context** |
| Could it not have known it was wrong? | build a check | **harness** |
| Did it stop too early, or never stop? | fix the stop condition | **loop** |
| Did the wrong specialist take the work? | routing | **graph** |

Cheapest first. `[INFERENCE]` Most reported "prompting problems" are rows 2 and 3. The tell is
whether a *better sentence* could plausibly have fixed it — if not, no amount of prompt work
will.

---

## The four-layer naming stack `[FACT]`

The clearest framing in the literature, from [[Source - Learn Harness Engineering Course]]
Lecture 14, crediting a thread by @rohit4verse:

| Layer | Shapes | Answers | Artefacts |
|---|---|---|---|
| **Prompt engineering** | the instruction | how do we tell the model what to do? | instructions, examples, constraints, roles, output formats |
| **Context engineering** | the information | what should it know before it decides? | documents, history, memory, tool definitions, environment state |
| **Loop engineering** | the runtime | how does it iterate until the goal is met? | observe, reason, act, inspect, update, stop condition |
| **Graph engineering** | the system | how do agents, loops, tools and evaluators work together? | nodes, edges, shared state, routing |

**Each layer stacks; none replaces the one before.**

- Finding context engineering did not stop anyone prompting. The loop just refreshes the prompt
  each turn as the environment moves.
- Building loops did not remove context work. Every round **reassembles** its context.
- At the graph layer all of it survives: every node has its own prompt, context, tools, memory
  and loop.

`[FACT]` And the course's own correction: **harness is missing from that list** because the
thread was telling a story about buzzwords. This vault places it beneath loop — see
[[The Unified Mental Model]].

---

## The one genuine disagreement `[FACT]`

Where the harness sits relative to context engineering is **not settled**:

| Claim | Source |
|---|---|
| The harness contains context engineering as a part | [[Source - Wikipedia Agent Harness]] |
| A coding-agent harness **is a specific form of** context engineering | [[Source - Harness Engineering for Coding Agent Users]] |

`[INFERENCE]` Both hold on different axes. Böckeler is right mechanically — controls reach the
agent by entering its context. Wikipedia is right architecturally — a sandbox, a permission
boundary and a test runner are not context. **Context engineering is the delivery mechanism;
the harness is larger than what it delivers.** Do not present either containment as settled.

---

## Is prompt engineering obsolete?

`[INFERENCE]` **No** — but its centre of gravity moved, and the move is the interesting part.

`[FACT]` Both OpenAI and Thoughtworks independently write remediation instructions **into
custom linter messages**. `[FACT]` Anthropic names "carefully craft your **agent-computer
interface** through thorough tool documentation" as one of three core principles, and titles an
appendix *"Prompt Engineering your Tools."*

So the highest-value prompt engineering is no longer in the chat box. It is in **error
messages, tool descriptions, test names, and check output** — text the agent reads at the
moment it matters, at zero cost when things are fine. See [[Feedback Quality]].

---

## Related

- [[Context Engineering]] · [[Context Window as a Budget]] · [[The Unified Mental Model]]
- [[Harness vs Loop vs Graph]] · [[Feedback Quality]] · [[Instruction File Design]]
- [[Comparisons MOC]]
