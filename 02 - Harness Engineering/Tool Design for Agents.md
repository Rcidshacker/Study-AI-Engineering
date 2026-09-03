---
title: Tool Design for Agents
aliases:
  - Agent-computer interface
  - ACI
  - End-to-End Verification
tags:
  - harness-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Tool Design for Agents

> [!abstract] One line
> A tool is an interface for a reader that reasons. Its description, its arguments, and above
> all **its output** are prompts you are writing in advance.

---

## Anthropic names it a core principle `[FACT]`

One of three:

> "Carefully craft your **agent-computer interface (ACI)** through thorough tool documentation
> and testing."

With an appendix titled *"Prompt Engineering your Tools"*, and the general instruction to focus
on "ensuring they provide an easy, well-documented interface for your LLM."

`[INFERENCE]` The ACI is the agent's equivalent of a UI, and it deserves the same care — with
one difference: a confused human asks; a confused agent guesses, plausibly.

---

## Prefer general tools over catalogues `[FACT]`

> "Harnesses can only execute the tools they have logic for. Instead of forcing users to build
> tools for every possible action, a better solution is to give agents a general purpose tool
> like **bash**… The model can design its own tools on the fly via code instead of being
> constrained to a fixed set of pre-configured tools."

`[INFERENCE]` A large catalogue of narrow tools costs context on **every** session — each
description is loaded before use — and still cannot cover the case you did not anticipate.
A general tool plus good scripts in the repo covers more, costs less, and lets the agent
compose. `[FACT]` Claude Code's answer to catalogue growth is the same shape: MCP "tool schemas
are deferred by default and load on demand via tool search."

---

## Prefer interfaces the model already knows `[INFERENCE]`

`[FACT]` The filesystem is "arguably the most foundational harness primitive" partly because
models "were trained on billions of tokens of how to use them."

Generalised: **files, bash, git, JSON, markdown, HTTP** are native. A bespoke API is a tax paid
in every session, in context and in error rate. When you must invent an interface, make it look
like something that already exists.

This is the same reasoning that leads OpenAI to favour "boring" technologies — "composability,
api stability, and representation in the training set."

---

## Output is the part that matters most

`[FACT]` Claude Code's docs: "Each tool use returns information that feeds back into the loop,
**informing Claude's next decision.**"

`[INFERENCE]` So tool output is not a log. It is the next turn's prompt, and it should be
written that way:

| Bad output | Better output |
|---|---|
| 800 lines of raw log | the failing lines, plus what to do |
| `exit 1` | what failed, where, and the fix |
| a wall of JSON | the fields that matter for the decision |
| silence on success | a one-line confirmation |

Two consequences worth acting on:

- **Verbosity is a context cost.** A command printing 800 lines can cost more than the
  instruction that invoked it. See [[Context Window as a Budget]].
- **Failure output is the highest-leverage text in your repo.** See [[Feedback Quality]].

---

## End-to-end verification tools

`[FACT]` The single documented fix for agents that pass unit tests and still ship broken
features: give them tools to **verify as a user would**. Anthropic used browser automation —
"do all testing as a human user would" — and it "dramatically improved performance." OpenAI
wired the **Chrome DevTools Protocol** into the runtime with skills for DOM snapshots,
screenshots and navigation.

`[FACT]` Claude Code ships this as bundled skills: `/run` ("launch and drive your app to see a
change working") and `/verify` ("confirm a code change does what it should, **without falling
back to tests or type checks**").

`[FACT]` And the honest limit: Claude "can't see browser-native alert modals through the
Puppeteer MCP, and features relying on these modals tended to be buggier as a result."

`[INFERENCE]` Every verification tool has a blind spot. Find yours, and write it in the
instruction file — an agent that knows a check is partial behaves better than one that assumes
it is complete. See [[The Verification Gap]].

---

## The checklist

For each tool you expose:

- [ ] Does its **description** say when to use it *and when not to*?
- [ ] Are the arguments hard to get wrong?
- [ ] Does its output carry **only** what the next decision needs?
- [ ] Does a **failure** say what to do instead?
- [ ] Have you watched an agent actually use it? `[FACT]` Anthropic: "thorough tool
      documentation **and testing**."
- [ ] Does it duplicate something bash plus a script would do?

---

## Related

- [[Feedback Quality]] · [[Harness Components]] · [[Context Window as a Budget]]
- [[The Verification Gap]] · [[The Repository as System of Record]] · [[Claude Code Implementation Notes]]
- [[Source - Anthropic Building Effective Agents]] · [[Source - Anatomy of an Agent Harness]]
