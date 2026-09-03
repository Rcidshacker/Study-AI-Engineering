---
title: Inner Harness vs Outer Harness
aliases:
  - Outer harness
  - Inner harness
tags:
  - harness-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Inner Harness vs Outer Harness

> [!abstract] One line
> The vendor ships the **inner** harness; you build the **outer** one. Almost everything
> written about "harness engineering" for practitioners is about the outer harness, and
> confusing the two is why people talk past each other.

`[FACT]` The distinction is Birgitta Böckeler's, in
[[Source - Harness Engineering for Coding Agent Users]]:

> "In coding agents, part of the harness is already built in (e.g. via the system prompt, or
> the chosen code retrieval mechanism, or even a sophisticated orchestration system). But
> coding agents also provide us, their users, with many features to build an **outer harness**
> specifically for our use case and system."

She cheerfully concedes the metaphor breaks: *"Have you ever tried to put a harness on the
inside of a dog?"* — and keeps it anyway because it navigates the word's ambiguity.

---

## The split, for Claude Code

| | Inner harness (Anthropic's) | Outer harness (yours) |
|---|---|---|
| Instructions | the system prompt | `CLAUDE.md`, skills, output styles |
| Context | compaction strategy, retrieval, file-read heuristics | what you put in the repo; what you exclude |
| Tools | Read/Edit/Bash/Glob/Grep/Task and their descriptions | MCP servers, custom skills, scripts you expose via bash |
| Permissions | the permission system's existence | your allow/deny rules |
| Loop | the built-in agent loop | scripted loops around whole sessions |
| Orchestration | subagent spawning mechanics | which subagents exist and what they do |
| Feedback | tool results, error surfacing | your tests, linters, hooks, reviewer subagents |

**You cannot change the left column.** You can change every row on the right, and the right
column is where essentially all the leverage documented in
[[Source - OpenAI Harness Engineering]] and
[[Source - Anthropic Effective Harnesses for Long-Running Agents]] lives.

---

## Why the distinction earns its keep

**1. It settles arguments about who is responsible.** "The agent keeps editing files outside
the feature directory" is an outer-harness problem — you have no permission rule and no
structural test. It is not a model complaint.

**2. It tells you what transfers.** Outer-harness work is largely **portable across agents**:
a `feature_list.json`, an `init.sh`, a test suite, custom linters with agent-readable error
messages, an architecture-boundary test. Move to Codex tomorrow and most of it still works.
Inner-harness knowledge (how compaction triggers, which tool the model prefers) does not
transfer. `[INFERENCE]` Invest accordingly.

**3. It explains the `AGENTS.md` convergence.** Several vendors read the same instruction
file precisely because that file is *outer* harness — it belongs to the repo, not the tool.

**4. It bounds what a benchmark measures.** A model benchmark run through one inner harness
tells you little about your results through a different inner harness plus your outer one.
See [[Harness Beats Model Choice]].

---

## Where the boundary is genuinely blurry `[INFERENCE]`

- **Skills and plugins** are authored by you but executed by vendor machinery.
- **MCP servers** are an inner-harness *protocol* hosting outer-harness *capability*.
- **Hooks** are the clearest hybrid: a vendor-provided extension point whose entire content
  is yours. `[INFERENCE]` This is why hooks are the highest-leverage outer-harness surface in
  Claude Code — they let you insert deterministic control into the inner loop, which is
  otherwise closed to you.

---

## Practical consequence

When something goes wrong, ask **which harness** first:

```text
Can I fix this by changing a file in my repo, a permission rule, a hook,
a test, or a skill?
   yes → outer harness. Build the control. Stop complaining about the model.
   no  → inner harness. Your options are: work around it, choose a different
         agent, or file feedback. Do not build elaborate compensation for it
         without first checking it is still true — inner harnesses change fast.
```

---

## Related

- [[Harness Engineering]] · [[Harness Components]] · [[Guides and Sensors]]
- [[Claude Code as a Harness]] · [[Harness Beats Model Choice]]
- [[Source - Harness Engineering for Coding Agent Users]] · [[Source - Wikipedia Agent Harness]]
