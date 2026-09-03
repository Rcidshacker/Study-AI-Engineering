---
title: Agent State
aliases:
  - State management for agents
  - Agent memory
tags:
  - harness-engineering
  - agent-state
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Agent State

> [!abstract] One line
> The model has none. Every notion of "where we are" is something the environment supplies,
> which makes state a **design decision** rather than a property of the system.

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose. This version is grounded in Anthropic's
> long-running-agents report, the Wikipedia article's framing, and Ralph's actual files.

---

## The premise `[FACT]`

[[Source - Wikipedia Agent Harness]]: an LLM "is stateless and, unaided, produces only text."

[[Source - Anthropic Effective Harnesses for Long-Running Agents]]: "each new session begins
with no memory of what came before… engineers working in shifts, where each new engineer
arrives with no memory of what happened on the previous shift."

`[INFERENCE]` Two consequences people conflate and should not:

- **Within a session**, the transcript *is* the state. It is finite, it fills, and everything
  in it competes for attention. See [[Context Window as a Budget]].
- **Across sessions**, there is nothing. Not a smaller amount — nothing. Continuity is entirely
  manufactured by the harness.

---

## The four tiers

| Tier | Lives in | Survives | Cost to read |
|---|---|---|---|
| **Working** | the context window | one turn | already loaded |
| **Session** | transcript, compaction, resume | one session, lossily | free, but not inspectable |
| **Project** | files in the repo, git | forever | one read |
| **Cross-project** | user-level instructions, accumulated memory | forever | loaded every session |

`[FACT]` Claude Code documents surfaces at every tier: the context window, sessions with
resume/branch/fork and checkpointing, `CLAUDE.md` and repo files, and auto memory that
"accumulates learnings automatically."

`[INFERENCE]` **Project tier is where to put anything that matters.** It is the only tier that
is simultaneously durable, human-readable, diffable, reviewable, and portable to a different
agent. Session-tier state is a convenience; a committed progress file is an artefact.

---

## Why compaction is not a state strategy `[FACT]`

> "Compaction isn't sufficient… it doesn't always pass perfectly clear instructions to the
> next agent."

`[INFERENCE]` Compaction is a *safety net for overflow*, not memory. It is lossy in ways you
cannot predict or inspect, and you find out what it dropped by watching the agent redo work.
If your design depends on something surviving, that something belongs in a file.

Claude Code exposes `PreCompact` and `PostCompact` hooks. `[INFERENCE]` `PreCompact` is
precisely the seam at which to write critical state to disk before it is summarised away.

---

## The artefacts that work

`[FACT]` The set Anthropic's initializer agent creates on the first run:

| Artefact | Role |
|---|---|
| `init.sh` | how to bring the environment up |
| `claude-progress.txt` | a log of what agents have done |
| a feature list (JSON) | what "all of done" means |
| an initial git commit | a baseline to recover to |

And the arrangement Ralph adds — a two-tier progress file with consolidated patterns at the
**top**, because a fresh agent reads from the top. Full treatment in [[External State]].

`[FACT]` The empirical detail worth respecting: **JSON over Markdown** for anything the agent
must not rewrite, because "the model is less likely to inappropriately change or overwrite
JSON files."

---

## Git is state *and* undo

`[FACT]` Anthropic: descriptive commits let the model "revert bad code changes and **recover
working states** of the code base." Trivedy: "Git adds versioning to the filesystem so agents
can track work, rollback errors, and branch experiments."

`[INFERENCE]` This dual role is why git outranks every other state store. It is the only one
that both records what happened and bounds the damage of what happened wrongly. An unattended
loop without commits has no floor.

---

## The design rules

1. **Externalise anything that must survive a context window.**
2. **Structured status in JSON; narrative in markdown.**
3. **Append logs; never let the agent rewrite history.**
4. **The agent may change status, not scope.**
5. **Commit it** — state outside git cannot be reviewed, diffed, or rolled back.
6. **Make the first read cheap.** Three files, not thirty. See [[Coding Agent Startup Flow]].
7. **Order is interface.** What a fresh agent reads first is a design choice.

---

## The test

> Kill the session mid-task. Start a fresh one with no history. Can it determine what was
> happening, what is done, what is next, and whether the tree is healthy — from files alone?

`[INFERENCE]` If not, you have a long conversation rather than state. Every unattended
iteration is exactly this scenario.

---

## Related

- [[External State]] · [[Context Window as a Budget]] · [[The Repository as System of Record]]
- [[Feature List as Harness Primitive]] · [[Clean State Ritual]] · [[Harness Components]]
- [[Source - Anthropic Effective Harnesses for Long-Running Agents]] · [[Ralph Loop]]
