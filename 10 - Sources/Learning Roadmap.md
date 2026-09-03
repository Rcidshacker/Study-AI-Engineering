---
title: Learning Roadmap
aliases:
  - What should I learn next?
  - Claude Code Learning Path
tags:
  - roadmap
  - learning
  - evergreen
status: evergreen
confidence: medium-high
created: 2026-09-04
updated: 2026-09-04
---

# Learning Roadmap

> [!abstract] The question
> *If I want to become genuinely good at building reliable AI coding-agent systems with
> Claude Code, what should I learn and build?*

---

## The proposed progression, and how the research changes it

The eight-level progression in the original brief was:

```text
Basic Claude Code → Context + Instructions → Skills + Tools → Harness → Loops
→ Verification + Evaluation → Graph / Orchestration → Production
```

`[INFERENCE]` Two corrections, both grounded in the sources:

**1. Verification moves from level 6 to level 2.** Every documented failure mode —
[[False Completion]], one-shotting, weak end-to-end testing, this vault's own
[[Research Integrity in Agent-Assisted Research|fabrication incident]] — is a verification
failure. `[FACT]` [[Source - Learn Harness Engineering Course]] states the feedback subsystem
"usually has the lowest investment and highest return." Learning skills and tools before you
can tell whether the agent is right teaches you to move faster in an unknown direction.

**2. "Skills + Tools" is not a level; it is a technique used at several levels.** A skill is a
delivery mechanism for instructions. Treating it as a rung encourages collecting extensions
rather than closing gaps — the opposite of the docs' own advice to add extensions "as specific
triggers come up."

The corrected progression, with verification pulled forward and skills folded in:

```text
   1  Use it well                      ── a week
   2  Verification first               ── a week      ← the rung people skip
   3  Instructions and context         ── a week
   4  The full harness                 ── two weeks
   5  Your first loop                  ── a week
   6  Autonomy and safety              ── two weeks
   7  Graphs, if you need one          ── a week
   8  Production                       ── ongoing
```

Each level below has an **exit test** — a thing you can do, not a thing you have read.

---

## Level 1 — Use it well

**Goal:** a working feel for the agentic loop and where it breaks.

Read [[Claude Code]] and the official *How Claude Code works*. Use it daily on real work with
**no configuration at all**. Keep a plain text file of every time it does something wrong.

`[INFERENCE]` That file is the most valuable artefact you will produce this month. Every later
level is a response to something in it, and a harness built from a checklist instead of from
observed failures is the thing all three primary sources warn against.

**Exit test:** ten entries, each naming a specific failure.

---

## Level 2 — Verification first

**Goal:** make it possible for the agent to be told it is wrong.

Read: [[The Verification Gap]] · [[False Completion]] · [[Generator Evaluator Separation]].

Build:
1. **One command** that verifies your project (`make check` / `npm run check`).
2. Put it in `CLAUDE.md`.
3. Take your three commonest failures and **rewrite the error messages** they produce to say
   what to do instead — see [[Feedback Quality]].

**Project:** take a repo with weak tests and add end-to-end tests for the three paths a user
actually takes. Not unit tests. `[FACT]` Anthropic's agents passed unit tests and curl checks
and still shipped features that did not work end-to-end.

**Exit test:** you can name a change that breaks your app and a command that catches it.

---

## Level 3 — Instructions and context

**Goal:** a map, not a manual.

Read: [[Instruction File Design]] · [[Context Window as a Budget]] ·
[[Source - OpenAI Harness Engineering]] (in full — it is the best single read in the field).

Build a `CLAUDE.md` under 200 lines: commands, stack, hard constraints, pointers. Move
anything task-specific into a skill or a path-scoped rule. Then start `docs/` as the system of
record.

**Project:** take an existing 400-line instruction file and halve it, moving the rest into
skills and rules. Measure whether adherence gets *better*. `[INFERENCE]` It usually does, and
experiencing that is what makes the "too much guidance becomes non-guidance" finding real
rather than theoretical.

**Exit test:** a new contributor could run your project from `CLAUDE.md` alone.

---

## Level 4 — The full harness

**Goal:** all five subsystems present.

Read: [[Harness Components]] · [[Guides and Sensors]] · [[Inner Harness vs Outer Harness]] ·
[[Source - Harness Engineering for Coding Agent Users]].

Build, following [[Coding Agent Harness]]: `init.sh`, `feature_list.json`, `progress.md`, a
`PostToolUse` hook running your checks, a permission allowlist.

Then audit with the 2×2: which of your controls are guides, which are sensors, which are
computational, which inferential? **Every empty quadrant is a real gap.**

**Project:** run [[Harness Ablation Testing]] — remove one subsystem at a time, on a fixed
task, and see what breaks. This is the exercise that converts belief into knowledge.

**Exit test:** kill a session mid-task; a fresh one recovers position from files alone.

---

## Level 5 — Your first loop

**Goal:** get outside the loop.

Read: [[Loop Engineering]] · [[Loop Types]] · [[Stopping Conditions]] ·
[[Inner Loops and Outer Loops]].

Build [[Autonomous Test Fixer]] — start with a `Stop` hook, then try `/goal`, then write the
external shell-loop version yourself. `[INFERENCE]` **Write the shell loop even though the
product has the feature.** Twenty lines of bash teaches you what `/goal` is doing; using
`/goal` teaches you nothing about loops.

Read `ralph.sh` and `default.py` from [[GitHub - snarktank ralph]] and
[[GitHub - SWE-agent mini-swe-agent]]. Both are short. Read them completely.

**Exit test:** your loop has four distinct exit statuses and reports which one fired.

---

## Level 6 — Autonomy and safety

**Goal:** run unattended without dread.

Read: [[Sandboxing and Permissions]] · [[Worktree Isolation]] · [[Clean State Ritual]] ·
[[Source - Anthropic Effective Harnesses for Long-Running Agents]].

Build: worktree or container isolation; a clean-state check at session end; escalation paths
that produce a written question rather than a guess; budgets on everything.

**Project:** run a loop overnight on a real backlog in an isolated worktree. Read the whole
log in the morning. `[INFERENCE]` Reading a full unattended transcript is the single most
educational hour in this roadmap — it is where the gap between what you *think* your harness
constrains and what it *actually* constrains becomes visible.

**Exit test:** you would let it run overnight on a repo you care about, and can say exactly
why that is safe.

---

## Level 7 — Graphs, if you need one

**Goal:** know when *not* to.

Read: [[Graph Engineering]] · [[Graph vs Workflow]] · [[Claude Code Graphs]] ·
[[Graph Engineering Origin and Fact-Check]] · Anthropic's *Building Effective Agents*.

Build exactly one: **maker/checker**, where a reviewer subagent with a fresh context validates
the main agent's work, fired by a hook. That is the graph with a genuine structural
justification. See [[Generator Evaluator Separation]].

**Project:** an audit workflow — one pass produces findings, a second set of agents
adversarially verifies each one. Then measure how many findings survive verification.
`[INFERENCE]` The survival rate is the most sobering number you will generate in this
roadmap, and it is the empirical case for the whole verification layer.

**Exit test:** you can name three tasks where a graph would be the *wrong* answer, and say
what to build instead.

---

## Level 8 — Production

**Goal:** it stays good.

Read: [[Harness Debt and Garbage Collection]] · [[Continuous Drift Sensors]] ·
[[Production Coding Agent]].

Build: garbage-collection tasks on a cadence; drift sensors outside the change lifecycle;
observability the agent itself can query; harness audits as a recurring chore.

`[FACT]` OpenAI's team spent **20% of every week** cleaning up "AI slop" before they automated
it into background tasks that scan for deviations and open targeted refactoring PRs. Plan for
this from the start — it is not an optional late-stage concern.

**Exit test:** your harness has an owner, a review cadence, and a debt tracker — the same
things your code has.

---

## The three habits that matter more than the levels

`[INFERENCE]` If you retain nothing else:

1. **Fix the class, not the instance.** Third occurrence of a mistake ⇒ build a control. See
   [[Fix the Class Not the Instance]].
2. **Anything the agent reads is a prompt.** Error messages, test names, file names, log
   lines. See [[Feedback Quality]].
3. **Verify before you trust, including your own research.** The cheap check almost always
   exists. See [[Research Integrity in Agent-Assisted Research]].

---

## What to read, in order

1. [[Source - OpenAI Harness Engineering]] — the best single read
2. [[Source - Anthropic Effective Harnesses for Long-Running Agents]] — the failure modes
3. [[Source - Harness Engineering for Coding Agent Users]] — the best framework
4. `default.py` in [[GitHub - SWE-agent mini-swe-agent]] — what a loop actually is
5. `ralph.sh` + `CLAUDE.md` in [[GitHub - snarktank ralph]] — what an outer loop actually is
6. [[Source - Learn Harness Engineering Course]] lectures 02, 13, 14 — the synthesis

`[INFERENCE]` Six items, all readable in a weekend. The field is a year old; there is no large
literature to work through. The scarce thing is not reading material — it is **hours spent
reading your own agents' transcripts**.

---

## Related

- [[AI Engineering MOC]] · [[Harness Loop Graph MOC]] · [[The Unified Mental Model]]
- [[Coding Agent Harness]] · [[Autonomous Test Fixer]] · [[Sources MOC]]
