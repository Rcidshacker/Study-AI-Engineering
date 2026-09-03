---
title: GitHub - shareAI-lab learn-claude-code
aliases:
  - learn-claude-code
tags:
  - github
  - repository
  - harness-engineering
  - claude-code
  - evergreen
repo: shareAI-lab/learn-claude-code
url: https://github.com/shareAI-lab/learn-claude-code
stars: 75990
license: MIT
language: Python
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# GitHub — shareAI-lab/learn-claude-code

## Overview

*"Learn Claude Code — Harness Engineering for Real Agents."* A seventeen-module course that
builds a Claude-Code-like agent from scratch in Python, one subsystem per module.

## GitHub

`https://github.com/shareAI-lab/learn-claude-code` — GitHub API, **2026-09-04**:

| Field | Value |
|---|---|
| Stars / forks | 75,990 / 12,236 |
| Language / licence | Python / MIT |
| Created | 2025-06-29 |
| Last push | 2026-08-26 |
| Open issues | 34 |
| Topics | agent, agent-development, ai-agent, claude, claude-code, educational, llm, python, teaching, tutorial |

## Why it matters

`[INFERENCE]` It is the **build-it-yourself** counterpart to reading a finished agent. Where
[[GitHub - SWE-agent mini-swe-agent]] shows you the smallest thing that works,
this shows you the order in which capabilities get added — and the module names are, in effect,
an independently-derived component inventory. Compare them against
[[Harness Components]] and see how closely they line up.

## Architecture — the seventeen modules `[FACT — directory listing]`

```text
s01_agent_loop          s07_skill_loading       s13_agent_teams
s02_tool_use            s08_context_compact     s14_mcp_plugin
s03_permission          s09_memory              s15_integrated_harness
s04_hooks               s10_task_system         s16_workflow_runtime
s05_todo_write          s11_background_tasks    s17_goal_loop
s06_subagent            s12_cron_scheduler
```

Plus `agents/`, `skills/`, `docs/`, `tests/`, `web/`.

`[INFERENCE]` Read that ordering as an argument. It starts at the loop and tools (s01–s02),
reaches **permissions before hooks** (s03–s04), spends s07–s09 on context and memory, and only
reaches multi-agent work at s06 and s13. The autonomy layer — background tasks, scheduling,
goal loops — arrives **last**, at s11, s12 and s17. That is the same investment order this
vault recommends in [[When Not to Build a Harness]], arrived at independently.

## The framing `[FACT — README]`

> "**Agency — the capacity to perceive, reason, and act — comes from model training, not from
> external code orchestration.** But a working agent product needs both the model and the
> harness. The model is the driver. The harness is the vehicle. This repository teaches you how
> to build the vehicle."

`[INFERENCE]` A useful corrective to over-claiming. The harness does not make the model smarter;
it makes the model's existing capability **usable and verifiable**. That is compatible with
[[Harness Beats Model Choice]] — which claims the harness dominates *outcome variance on long
tasks*, not that it adds intelligence.

The README supports this with a documented history of learned agency — DQN on Atari (2013,
*Nature* 2015), OpenAI Five vs. OG (2019), AlphaStar reaching Grandmaster (2019) — each
citing a primary source. `[FACT]` The citations are given; I did not follow them.

## What to study

1. **`s01_agent_loop`**, then compare with `mini-swe-agent`'s `default.py`. Two independent
   minimal loops.
2. **`s03_permission`** — permissions as a first-class subsystem, not a nuisance. See
   [[Sandboxing and Permissions]].
3. **`s08_context_compact` and `s09_memory`** — the two hardest problems, separated. See
   [[Context Window as a Budget]] and [[Agent State]].
4. **`s15_integrated_harness`** — how the pieces compose.
5. **`s17_goal_loop`** — a goal loop you can read, next to
   [[Claude Code Loops|the product feature]].

## Limitations

- `[CAUTION]` **Teaching code, not production code.** Read it to understand mechanisms.
- Its model of Claude Code is a reconstruction, not documentation. For capability claims use
  [[Claude Code]] and the official docs.
- 34 open issues; primary docs are multilingual with English as one of three READMEs.

## Related

- [[GitHub - SWE-agent mini-swe-agent]] · [[Harness Components]] · [[Agent Loops]]
- [[Learning Roadmap]] · [[Repository Index]]
