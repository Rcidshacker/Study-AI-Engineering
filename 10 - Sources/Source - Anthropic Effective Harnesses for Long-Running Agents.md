---
title: Source - Anthropic Effective Harnesses for Long-Running Agents
aliases:
  - Effective harnesses for long-running agents
  - Anthropic long-running agents
tags:
  - source
  - primary-source
  - harness-engineering
  - anthropic
  - claude-code
source-type: engineering-blog
author: Anthropic (Engineering at Anthropic)
publisher: Anthropic
published: 2025-11-26
url: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
reliability: high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Anthropic Effective Harnesses for Long-Running Agents

> [!info] Verification
> Fetched and read directly on 2026-09-04. HTTP 200. Published **Nov 26, 2025** —
> **two and a half months before** [[Source - OpenAI Harness Engineering]]. This matters
> for [[Lineage of the Word Harness]]: Anthropic was using "harness" in this sense first,
> though it was OpenAI's February 2026 post that attached the phrase *harness engineering*
> to it and made it a discipline name.

## What it is

A report on Anthropic's internal experiments getting the Claude Agent SDK to make
consistent progress on a task spanning **many context windows** — specifically, building a
production-quality clone of claude.ai from a single high-level prompt.

The framing metaphor is the most useful one in the literature:

> "Imagine a software project staffed by engineers working in shifts, where each new
> engineer arrives with no memory of what happened on the previous shift."

## The central negative result `[FACT]`

> "Compaction isn't sufficient."

Even a frontier model (the post names **Opus 4.5**) running on the Claude Agent SDK in a
loop across multiple context windows **will fall short** of building a production-quality
web app from a high-level prompt alone. This is the single most important sentence for
anyone who believes better models remove the need for a harness. See
[[Harness Beats Model Choice]].

## The two named failure modes `[FACT]`

These are the canonical failure taxonomy for long-running coding agents.

### 1. One-shotting

The agent tries to do too much at once, runs out of context mid-implementation, and leaves
the next session with a feature half-implemented and undocumented. The next agent must
guess what happened and spend substantial time getting the app working again.
**Compaction does not fix this** — it "doesn't always pass perfectly clear instructions to
the next agent."

### 2. Premature victory

> "After some features had already been built, a later agent instance would look around,
> see that progress had been made, and declare the job done."

See [[False Completion]]. This is the failure the entire feature-list mechanism exists to
prevent.

## The two-agent solution `[FACT]`

| Agent | Runs | Job |
|---|---|---|
| **Initializer agent** | first session only, with a *different prompt* | Set up `init.sh`, `claude-progress.txt`, a comprehensive feature list, and an initial git commit |
| **Coding agent** | every subsequent session | Make incremental progress on **one feature**, then leave structured updates and a clean state |

The post notes this "different prompt for the very first context window" pattern was already
recommended in Anthropic's Claude 4 prompting guide. See [[Initialization as a Phase]].

## The feature list `[FACT]`

The highest-leverage artifact in the whole design.

- The initializer expands the user's prompt into a **comprehensive file of feature
  requirements**. For the claude.ai clone this was **over 200 features**.
- Each is an *end-to-end user-visible* description with explicit steps, e.g. "a user can
  open a new chat, type in a query, press enter, and see an AI response."
- All start marked `"passes": false`, so later agents have a clear outline of what full
  functionality means.

```json
{
  "category": "functional",
  "description": "New chat button creates a fresh conversation",
  "steps": [
    "Navigate to main interface",
    "Click the 'New Chat' button",
    "Verify a new conversation is created",
    "Check that chat area shows welcome state",
    "Verify conversation appears in sidebar"
  ],
  "passes": false
}
```

Two implementation details are load-bearing and easy to miss:

1. **Coding agents may only change the `passes` field.** Reinforced with strongly-worded
   instruction: *"It is unacceptable to remove or edit tests because this could lead to
   missing or buggy functionality."*
2. **JSON, not Markdown.** `[FACT]` "the model is less likely to inappropriately change or
   overwrite JSON files compared to Markdown files." A rare, concrete, empirical
   file-format finding — see [[Feature List as Harness Primitive]].

## Clean state `[FACT]`

The post defines it precisely: **the kind of code that would be appropriate for merging to
a main branch** — no major bugs, orderly and well-documented, and a developer could start a
new feature without first cleaning up an unrelated mess.

Mechanisms: commit to git with descriptive messages, and write progress summaries. Git also
gives the agent a way to **revert bad changes and recover working states**.
See [[Clean State Ritual]].

## Testing: the verification gap `[FACT]`

> "Claude tended to make code changes, and even do testing with unit tests or curl commands
> against a development server, but would fail to recognize that the feature didn't work
> end-to-end."

The fix was explicit prompting to **use browser automation tools and test as a human user
would** (Puppeteer MCP in their setup). This "dramatically improved performance."

Honest limits are stated: Claude cannot see browser-native alert modals through the Puppeteer
MCP, and features relying on those modals were buggier as a result. See
[[The Verification Gap]] and [[End-to-End Verification]].

## The startup ritual `[FACT]`

Every coding agent session begins by getting its bearings:

1. Run `pwd` to see the working directory.
2. Read the git log and progress files.
3. Read the feature list and choose the highest-priority feature not yet done.
4. Run `init.sh` to start the dev server, then run a basic end-to-end check *before*
   implementing anything new.

The rationale for step 4 is subtle and important: if the app was left broken, starting a new
feature "would likely make the problem worse." See [[Coding Agent Startup Flow]].

## How to use this source

- Pair it with [[Source - OpenAI Harness Engineering]]. Anthropic's is about **continuity
  across sessions**; OpenAI's is about **throughput and repo legibility**. They agree on
  the substrate (repo as memory, mechanical verification) and differ in emphasis.
- Everything here is implementable in [[Claude Code]] with a `CLAUDE.md`, a
  `feature_list.json`, an `init.sh` and a progress file. See [[Autonomous Test Fixer]].
- `[FACT]` The post calls the Claude Agent SDK "a powerful, general-purpose agent harness" —
  Anthropic's own usage of the word in the [[Harness Engineering]] sense.

## Related

- [[Harness Engineering]] · [[Agent State]] · [[False Completion]] · [[Feature List as Harness Primitive]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Anthropic Building Effective Agents]]
- [[Sources MOC]]
