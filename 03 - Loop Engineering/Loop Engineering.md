---
title: Loop Engineering
aliases:
  - Loop design
  - Designing loops that prompt agents
tags:
  - loop-engineering
  - evergreen
status: evergreen
confidence: medium-high
verified: 2026-09-04
created: 2026-09-03
updated: 2026-09-04
---

# Loop Engineering

> [!abstract] One line
> Replacing yourself as the person who prompts the agent. You design the system that does it
> instead.

> [!note] Rewritten 2026-09-04
> The earlier version was uncited generic prose. This version is grounded in Osmani's posts,
> the harness-engineering course, the official Claude Code documentation, and two loop
> implementations I read in full.

---

## The definition `[FACT]`

Addy Osmani, who named the discipline:

> "**Loop engineering is replacing yourself as the person who prompts the agent. You design
> the system that does it instead.**"

The page's own summary: "You don't really need to be good at prompting anymore. The thing to
get good at is the loop that does the prompting for you."

`[FACT]` Two related posts with dates I verified from the site feed: *Own the Outer Loop*
(15 Jul 2026) and *Practical Loop Engineering* (14 Aug 2026). The date of the original
*Loop Engineering* post is `[UNVERIFIED]` — see [[Source - Addy Osmani Loop Engineering]].

---

## Why it emerged when it did `[FACT — as reported]`

In one week of June 2026, three practitioners independently said the same thing: Peter
Steinberger ("you should be designing loops that prompt your agents"), Boris Cherny of
Anthropic ("I don't prompt Claude anymore… My job is to write loops"), and Osmani, who gave it
a name.

`[UNVERIFIED]` The specific quotes, view counts, and Cherny's autonomy figures (259 merged
PRs, >80% of production code, 76% success rate) come to me second-hand via
[[Source - Learn Harness Engineering Course]]. I did not reach the primary recordings. **Do
not quote the numbers.**

`[FACT]` What I *can* corroborate independently: `cobusgreyling/loop-engineering` was created
**2026-06-09**, days after the reported naming — consistent with the timeline.

Osmani's explanation of the timing is the substantive part:

> "A year ago if you wanted a loop you wrote a pile of bash and you maintained that pile
> forever… Now the pieces just ship inside the products."

`[INFERENCE]` The discipline is a year old not because the idea is new but because scheduling,
worktrees, subagents and skills became first-class features, so composing them stopped being a
maintenance burden.

---

## The three parts of any loop `[FACT]`

1. **A goal** — the end state, not the next step.
2. **A verification method** — how the end state is checked.
3. **A stopping condition** — including the budget for giving up.

> "More complex loops just add parts like scheduling, parallelism, isolation, and memory on
> top of these same three fundamentals."

`[INFERENCE]` If you cannot write all three down, you do not have a loop. You have a prompt
that repeats. **Write the shell command that proves you are done** before writing any loop.

---

## The building blocks `[FACT]`

Osmani decomposes a loop into five components plus a memory layer that is *not* a peer of the
others:

```text
Automations   scheduled or event triggers — the heartbeat
Worktrees     parallel isolation
Skills        codified project knowledge
Connectors    external tool access
Sub-agents    the maker/checker split
─────────────────────────────────────────
External State   the spine everything else rests on
```

`[NOTE]` The five-plus-one *shape* is confirmed by the post's own description; the component
*names* I read via the course. Verify before quoting them.

The asymmetry is the point: models forget everything between runs, so memory must live on
disk. See [[External State]].

---

## The four stages that produced autonomous loops `[FACT]`

1. **Manual prompting** — you are the scheduler.
2. **Long multi-step prompts** — several steps per turn, but it drifts and stalls.
3. **Self-reflection** — it decomposes and retries alone. But *when does it stop?*
   "Agents declare victory far too easily."
4. **Independent stopping judgment** — take "is it done?" away from the agent doing the work.

> "The person writing the code can't grade their own homework."

Stage 4 is the whole discipline. See [[Generator Evaluator Separation]] and
[[False Completion]].

---

## Getting outside the loop

```text
BEFORE   you → prompt → agent → output → you inspect → you prompt again → …
         your throughput is the system's throughput

AFTER    trigger → discover work → dispatch → verify → persist → next
                                                        └→ escalate → you
         you define the goal and stop condition before, review after,
         adjust the rules when it drifts
```

See [[Inner Loops and Outer Loops]] and [[Loop Types]] for the taxonomy.

---

## The four silent costs `[FACT — reported]`

Loops accelerate output *and* risk. Named costs that sharpen the longer a loop runs:

1. **Verification debt** — output outpacing confirmation
2. **Comprehension rot** — a codebase nobody has read
3. **Cognitive surrender** — approving because checking is tiring
4. **Token blowout** — cost scaling with iterations, not value

`[INFERENCE]` Costs 1 and 3 are the same failure at different layers, and together they are the
reason [[Generator Evaluator Separation]] is non-negotiable: if the only reviewer is a tired
human, effective verification approaches zero as throughput rises.

---

## Where to start

[[Autonomous Test Fixer]] — the one loop whose judge already exists and is perfect. Then read
`ralph.sh` and `default.py` (both short, both read in full for this vault). Then
[[Claude Code Loops]] for the surfaces.

`[INFERENCE]` Write the shell loop yourself even though the product ships the feature. Twenty
lines of bash teaches you what a goal loop is; using one teaches you nothing about loops.

---

## Related

- [[Loop Types]] · [[Stopping Conditions]] · [[Inner Loops and Outer Loops]] · [[Ralph Loop]]
- [[Generator Evaluator Separation]] · [[External State]] · [[Loop Failure Modes]]
- [[Claude Code Loops]] · [[Source - Addy Osmani Loop Engineering]] · [[The Unified Mental Model]]
