---
title: Agent Loops
aliases:
  - The agentic loop
  - ReAct loop
  - Agent Loop Patterns
tags:
  - loop-engineering
  - agent-engineering
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Agent Loops

> [!abstract] One line
> An agent is a model in a `while` loop with tools. Everything else is detail — and the detail
> that matters most is **how the loop learns it was wrong**.

> [!note] Rewritten 2026-09-04
> The earlier version of this note was uncited generic prose. This version is grounded in
> source I read: the official Claude Code documentation, Anthropic's *Building Effective
> Agents*, and the ~100-line agent in `mini-swe-agent`.

---

## The loop, as the vendors describe it

`[FACT]` **Claude Code's documentation** names three phases:

> "When you give Claude a task, it works through three phases: **gather context**, **take
> action**, and **verify results**. These phases blend together."

And, importantly for this vault, it names the thing itself:

> "**Claude Code serves as the agentic harness around Claude**: it provides the tools, context
> management, and execution environment that turn a language model into a capable coding
> agent."

`[FACT]` **Anthropic's *Building Effective Agents***, on agents generally:

> "They are typically just LLMs using tools based on environmental feedback in a loop."

`[FACT]` **LangChain**, naming the pattern: "The main agent execution pattern today is a
**ReAct loop**, where a model reasons, takes an action via a tool call, observes the result,
and repeats in a while loop."

`[FACT]` **ReAct is peer-reviewed prior art** — [[Source - Wikipedia Agent Harness]] records it
as the origin of the reason/act alternation, alongside **Toolformer** for tool calling. The
loop is not a 2026 invention.

---

## The loop, as actually written `[FACT — read from source 2026-09-04]`

From `mini-swe-agent`, which scores >74% on SWE-bench Verified in ~100 lines:

```python
while True:
    try:
        self.step()                       # step() = execute_actions(query())
        self.n_consecutive_format_errors = 0
    except FormatError as e:      ...      # count; exit if too many in a row
    except InterruptAgentFlow as e: self.add_messages(*e.messages)
    except Exception as e:        self.handle_uncaught_exception(e); raise
    finally:
        self.save(self.config.output_path)   # trajectory saved EVERY step
    if self.messages[-1].get("role") == "exit":
        break
```

Three observations that a prose description would hide:

1. **`step() = execute_actions(query())`.** Reason-then-act really is two function calls.
2. **Termination is a message role.** Every exit path — success, budget, timeout, repeated
   malformed output — converges on producing one `role: "exit"` message. A single uniform
   exit channel.
3. **The trajectory is saved in a `finally` block**, so it survives crashes. Observability is
   structural, not bolted on.

See [[GitHub - SWE-agent mini-swe-agent]].

---

## Why the loop matters more than the prompt

`[INFERENCE]` A single generation is a guess. A loop can be wrong and recover — **but only if
something in it can detect wrongness.** That is the whole distinction:

```text
prompt   →  output                       one guess, quality = model quality
loop     →  act → observe → correct      quality = model quality × feedback quality
```

`[FACT]` Anthropic states the requirement plainly: "it's crucial for the agents to gain
**'ground truth' from the environment at each step** (such as tool call results or code
execution) to assess its progress."

The corollary is the most important sentence in this note: **a loop without a real
verification signal does not converge, it just iterates.** It will still produce confident
output, and more of it. See [[The Verification Gap]].

---

## What every loop needs

| Part | Without it |
|---|---|
| A goal | nothing to converge toward |
| Actions | the model can only emit text |
| **Observation** | actions have no consequences it can see |
| **An independent judge** | it grades its own homework — [[Generator Evaluator Separation]] |
| Stopping conditions | it runs until the budget dies — [[Stopping Conditions]] |
| Durable state | each iteration starts from nothing — [[External State]] |

`[FACT]` Anthropic names stopping conditions explicitly, "such as a maximum number of
iterations, to maintain control," and warns that agent autonomy brings "higher costs, and the
potential for **compounding errors**."

---

## Loop layers

| Layer | Who owns it | Cycle |
|---|---|---|
| **Inner** | the agent vendor | gather context → act → verify, within one turn |
| **Outer** | **you** | trigger → discover work → dispatch → verify → persist → repeat |

You influence the inner loop only through instructions, available tools, and what tool results
say. You own the outer loop completely. See [[Inner Loops and Outer Loops]] and
[[Loop Engineering]].

---

## The human is in the loop by default `[FACT]`

Claude Code's docs: "You're part of this loop too. You can interrupt at any point to steer
Claude in a different direction."

`[INFERENCE]` That is the default state, and it is also the bottleneck. Every technique in
[[Loop Engineering]] is about moving the human from *inside* the loop to *outside* it —
defining the goal and the stopping condition beforehand, reviewing afterwards, adjusting the
rules when it drifts.

---

## Related

- [[Loop Engineering]] · [[Loop Types]] · [[Inner Loops and Outer Loops]] · [[Stopping Conditions]]
- [[The Verification Gap]] · [[Generator Evaluator Separation]] · [[Agent State]]
- [[Source - Anthropic Building Effective Agents]] · [[GitHub - SWE-agent mini-swe-agent]]
