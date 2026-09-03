---
title: Source - Addy Osmani Loop Engineering
aliases:
  - Osmani loop engineering
  - Own the Outer Loop
  - Practical Loop Engineering
tags:
  - source
  - primary-source
  - loop-engineering
source-type: engineering-blog
author: Addy Osmani
publisher: addyosmani.com
url: https://addyosmani.com/blog/loop-engineering/
reliability: high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Addy Osmani Loop Engineering

> [!info] Verification — read this before citing a date
> Verified on 2026-09-04:
> - `https://addyosmani.com/blog/loop-engineering/` returns **HTTP 200**, title
>   **"Loop Engineering | AddyOsmani.com"**. The rendered page byline reads **"Aug 2026"**.
> - The site RSS feed (`addyosmani.com/rss.xml`, 10 most recent items) independently
>   confirms two related posts with exact dates:
>   - **"Own the Outer Loop"** — Wed, **15 Jul 2026** — `/blog/own-the-outer-loop/`
>   - **"Practical Loop Engineering"** — Fri, **14 Aug 2026** — `/blog/practical-loop-engineering/`
>
> `[UNVERIFIED]` [[Source - Learn Harness Engineering Course]] dates the original
> *Loop Engineering* post to **7 June 2026**. I could not confirm that date: the post body
> is client-rendered and did not appear in the raw HTML, and the feed only retains ten
> items so the original may have aged out. **Do not cite 7 June 2026 as fact.** What is
> solid: Osmani is the person who named the concept, and the cluster of posts is mid-2026.

## Who

Addy Osmani — per his own site, over 14 years at Google leading developer experience across
Chrome and, more recently, AI (Gemini, coding agents), most recently as a Director at Google
Cloud AI. Author of the O'Reilly book *Beyond Vibe Coding*, described on the page as covering
"specs, harnesses, evals, context, and shipping production-grade software with AI."
`[FACT — from the page]`

## The definition `[FACT]`

The page's own meta description states the thesis compactly:

> "You don't really need to be good at prompting anymore. The thing to get good at is the
> **loop that does the prompting for you.** It's five building blocks plus s[tate]…"

As quoted in [[Source - Learn Harness Engineering Course]], the one-line definition is:

> **"Loop engineering is replacing yourself as the person who prompts the agent. You design
> the system that does it instead."**

That reframing — the human moves from *inside* the loop to *outside* it — is the whole
discipline. See [[Loop Engineering]] and [[Inner Loops and Outer Loops]].

## The five building blocks plus state `[FACT — five-plus-one structure confirmed by the page description; the specific names below are reported via the course]`

The course reports Osmani's decomposition as five components plus a memory layer that runs
through all of them:

| Block | Role |
|---|---|
| **Automations** | scheduled or event triggers — the heartbeat that makes it a loop rather than a one-off |
| **Worktrees** | parallel isolation so concurrent agents cannot collide |
| **Skills** | codified project knowledge the loop can reuse |
| **Connectors** | access to external tools and systems |
| **Sub-agents** | the maker/checker split |
| **External State** | *not a peer component* — the spine everything else depends on |

`[NOTE]` The "five plus one" shape is corroborated by the page's own description
("five building blocks plus s…"), which is why I record it with reasonable confidence
despite reading the block names second-hand. Verify the names against the live post before
quoting them.

The important structural claim is the asymmetry: **external state is not one of the five.**
Models forget everything between runs, so memory must live on disk — in markdown files,
issue trackers, kanban boards. See [[Agent State]] and [[External State]].

## The line about why now `[FACT — quoted in the course]`

> "A year ago if you wanted a loop you wrote a pile of bash and you maintained that pile
> forever and it was yours and only yours. Now the pieces just ship inside the products."

`[INFERENCE]` This is the practical reason loop engineering became a named discipline in
2026 rather than 2024: the primitives (scheduling, worktrees, sub-agents, skills) became
first-class product features, so composing them stopped being a maintenance burden. See
[[Claude Code Loops]].

## The inner/outer framing

"Own the Outer Loop" (15 Jul 2026, date verified) is the piece that most directly names the
distinction the user asked about. The inner loop is what the agent does within one session —
reason, act, observe. The outer loop is what *you* build around whole sessions — triggering,
work discovery, verification, state persistence, escalation. See
[[Inner Loops and Outer Loops]].

## How to use this source

- Osmani is the **naming** authority for loop engineering; he is not reporting a controlled
  experiment. Treat the framework as well-informed practitioner synthesis, not measurement.
- For *measured* outcomes of loop-driven development, go to
  [[Source - OpenAI Harness Engineering]] instead.
- Read the three posts in order: *Loop Engineering* → *Own the Outer Loop* →
  *Practical Loop Engineering*.

## Related

- [[Loop Engineering]] · [[Inner Loops and Outer Loops]] · [[External State]] · [[Stopping Conditions]]
- [[Source - Learn Harness Engineering Course]] · [[Sources MOC]]
