---
title: The Verification Gap
aliases:
  - Verification Gap
  - Agent Verification
  - Gulf of Evaluation
tags:
  - harness-engineering
  - verification
  - loop-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# The Verification Gap

> [!abstract] One line
> The agent cannot tell whether it succeeded. Everything else about agent unreliability is
> downstream of this.

---

## The gap, precisely

An agent can act. It usually cannot **observe the consequence of acting** at the level a user
would. It sees the tool result — "file written," "0 errors" — not the running product.

`[FACT]` The canonical documentation is from
[[Source - Anthropic Effective Harnesses for Long-Running Agents]]:

> "Claude tended to make code changes, and even do testing with unit tests or curl commands
> against a development server, but would fail to recognize that the feature **didn't work
> end-to-end**."

Note what this rules out: the agent *was* testing. Unit tests passed. Curl returned 200. The
feature was still broken. **Weak verification is not the same as none, and it is more
dangerous, because it produces confidence.**

---

## Why it produces the two big failure modes

| Failure | Mechanism |
|---|---|
| [[False Completion]] | Nothing in the environment contradicts "done," so "done" is the agent's honest belief |
| Silent regression | The check that would have caught it does not exist or does not run |

`[FACT]` The second Anthropic failure mode — a later agent "would look around, see that
progress had been made, and declare the job done" — is a verification failure, not a
motivation failure. There was no artefact that defined *what full functionality meant*. The
fix was the feature list. See [[Feature List as Harness Primitive]].

---

## Closing it: the four moves, in order of return

### 1. Define done as an artefact, not a sentence `[FACT]`

A checkable list of end-to-end, user-visible behaviours, all starting `false`. Anthropic used
**over 200** for a claude.ai clone, each with explicit steps. Agents may only flip the status
field, never edit or delete entries.

### 2. Verify the way a user would `[FACT]`

Unit tests and curl are not enough for user-facing behaviour. Anthropic's fix was browser
automation — "do all testing as a human user would" — which "dramatically improved
performance." [[Source - OpenAI Harness Engineering]] went further and wired the **Chrome
DevTools Protocol** into the agent runtime with skills for DOM snapshots, screenshots and
navigation, and had the agent **record video** of the failure and of the fix.

> [!note] Honest limits
> Anthropic reports that Claude could not see browser-native alert modals through the
> Puppeteer MCP, and features relying on those modals stayed buggier. Verification tooling
> has blind spots; know yours and note them in your instruction file.

### 3. Separate the checker from the maker `[FACT]`

Agents "confidently praise their own work." The producer and the judge must not share a
context. See [[Generator Evaluator Separation]].

### 4. Make failure legible `[FACT]`

Errors should carry their own remediation. See [[Feedback Quality]] and
[[Guides and Sensors]].

---

## Observability as verification `[FACT]`

The most advanced form documented: expose **logs, metrics and traces** to the agent through a
local, per-worktree, ephemeral observability stack. OpenAI's agents query logs with LogQL and
metrics with PromQL, which makes goals like *"ensure service startup completes in under
800ms"* or *"no span in these four critical user journeys exceeds two seconds"* directly
verifiable by the agent.

`[INFERENCE]` This is the general principle: **any property you want the agent to maintain
must be queryable by the agent.** Otherwise it is a wish, not a requirement. See
[[Agent Observability]].

---

## The diagnostic

Before blaming a model, run this:

```text
1. Is there a command that fails when the change is wrong?          no → build it
2. Does that command exercise the behaviour end-to-end?             no → extend it
3. Does the agent know to run it?                                   no → put it in CLAUDE.md
4. Does its failure message say what to do?                         no → rewrite the message
5. Is the thing that says "done" the same thing that did the work?  yes → split them
```

`[INFERENCE]` In my reading of the sources, most reported "the model is not good enough"
situations fail at step 1 or step 2.

---

## The framing worth keeping `[FACT]`

[[Source - Learn Harness Engineering Course]] frames this with Norman's gulfs: a **Gulf of
Execution** is the agent not knowing *how to act*; a **Gulf of Evaluation** is the agent not
knowing *whether it acted correctly*. Instructions and tools close the first. Only feedback
closes the second — and the second is the one that ships bugs.

---

## Related

- [[False Completion]] · [[Generator Evaluator Separation]] · [[Feedback Quality]]
- [[Feature List as Harness Primitive]] · [[End-to-End Verification]] · [[Agent Observability]]
- [[Harness Components]] · [[Guides and Sensors]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]] · [[Source - OpenAI Harness Engineering]]
