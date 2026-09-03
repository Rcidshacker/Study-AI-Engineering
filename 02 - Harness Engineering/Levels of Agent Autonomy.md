---
title: Levels of Agent Autonomy
aliases:
  - Human In The Loop
  - Autonomy ladder
  - Human oversight
tags:
  - harness-engineering
  - loop-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Levels of Agent Autonomy

> [!abstract] One line
> Autonomy is not a setting you choose. It is **earned**, one control at a time, and the human
> does not leave the loop so much as move to a different position in it.

---

## The ladder

`[INFERENCE]` Each rung requires everything below it. Skipping a rung is how unattended work
goes wrong.

| # | Level | Human does | Requires |
|---|---|---|---|
| 0 | Suggestion | reads, applies by hand | nothing |
| 1 | Supervised edit | approves each change | permissions |
| 2 | Supervised task | approves at checkpoints | instructions + tools |
| 3 | **Verified task** | reviews the result | **a real verification signal** |
| 4 | Unattended loop | reviews the output afterwards | + budgets, isolation, external state |
| 5 | Autonomous pipeline | reviews only escalations | + an independent judge, drift sensors |

**Rung 3 is the wall.** `[INFERENCE]` Levels 0–2 work because *you* are the sensor. Above that
the environment must supply the signal, and no amount of prompting substitutes. Most attempts
to jump from 2 to 4 fail here, and are misread as model failures. See [[The Verification Gap]].

---

## Level 5, as actually documented `[FACT]`

OpenAI's eleven-step sequence, from a single prompt:

1. Validate the current state of the codebase
2. Reproduce a reported bug
3. **Record a video demonstrating the failure**
4. Implement a fix
5. Validate the fix by driving the application
6. **Record a second video demonstrating the resolution**
7. Open a pull request
8. Respond to agent and human feedback
9. Detect and remediate build failures
10. **Escalate to a human only when judgment is required**
11. Merge the change

> [!warning] The caveat is part of the claim
> "This behavior depends heavily on the specific structure and tooling of this repository and
> **should not be assumed to generalize without similar investment** — at least, not yet."

`[INFERENCE]` Steps 1, 3 and 6 are the ones that make the rest defensible. Validating before
starting prevents compounding an existing breakage; recording video before and after produces
evidence a human can check in seconds rather than re-deriving. **Autonomy is bought with
evidence production**, not with confidence.

---

## Where the human goes

`[FACT]` Claude Code's docs describe the default: "You're part of this loop too. You can
interrupt at any point." `[FACT]` Anthropic's guidance on agents: they "pause for human feedback
at checkpoints or when encountering blockers."

`[INFERENCE]` As you climb, the human's four jobs stay constant while the position changes:

| At low autonomy | At high autonomy |
|---|---|
| approve each action | **set the goal and the stopping condition** |
| catch mistakes | **review outcomes and escalations** |
| supply missing context | **repair the environment when the agent struggles** |
| decide what is next | **prioritise, and write acceptance criteria** |

`[FACT]` OpenAI's version: humans "prioritize work, translate user feedback into acceptance
criteria, and validate outcomes. When the agent struggles, we treat it as a signal: identify
what is missing — tools, guardrails, documentation — and feed it back into the repository,
always by having Codex itself write the fix." See [[Fix the Class Not the Instance]].

---

## Escalation is a design feature `[FACT]`

"Escalate to a human **only when judgment is required**" — not when the task is hard, but when
it needs a decision the repository cannot supply.

`[INFERENCE]` A good escalation is a **written question with context**, left in a file: what
was attempted, what was observed, what the options are, what is blocked. A bad escalation is
silence, or a guess. The test: could you answer it in two minutes without re-reading the
transcript? If not, the escalation path is the thing to fix.

---

## The costs of climbing `[FACT — reported via Osmani]`

Four costs that sharpen with autonomy: **verification debt**, **comprehension rot**,
**cognitive surrender**, **token blowout**.

`[INFERENCE]` **Cognitive surrender is the dangerous one**, because it is invisible from the
inside: approving because checking is tiring looks identical to approving because it is
correct. It is the mechanism by which a nominal level 4 becomes an actual level 5 without
anyone deciding to climb — and without the controls level 5 requires.

The defence is structural, not personal: an independent judge, so that human review is a
second line rather than the only one. See [[Generator Evaluator Separation]].

---

## The readiness question

Before moving up a rung, ask: **what would go wrong here that nothing would catch?**
`[INFERENCE]` If you can name it, you are not ready — build the control. If you cannot name
anything, you have probably not looked hard enough; read one complete unattended transcript
first. See [[Agent Observability]].

---

## Related

- [[The Verification Gap]] · [[Generator Evaluator Separation]] · [[Stopping Conditions]]
- [[Sandboxing and Permissions]] · [[Worktree Isolation]] · [[Production Coding Agent]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Anthropic Building Effective Agents]]
