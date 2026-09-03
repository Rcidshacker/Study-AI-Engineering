---
title: Source - Wikipedia Agent Harness
aliases:
  - Wikipedia agent harness
  - Agent scaffolding
tags:
  - source
  - tertiary-source
  - harness-engineering
source-type: encyclopedia
author: Wikipedia contributors
publisher: English Wikipedia
url: https://en.wikipedia.org/wiki/Agent_harness
reliability: medium
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Wikipedia Agent Harness

> [!info] Verification
> Retrieved via the MediaWiki API on 2026-09-04; article extract read in full (~4,600 chars).
> Wikipedia is a **tertiary** source. It is useful here for one thing the primary sources
> cannot give: a neutral account of *contested attribution*. Every claim below should be
> re-checked against a primary source before you lean on it.

## The definition it settles on `[FACT]`

> "An agent harness, also known as **agent scaffolding**, is the software infrastructure
> surrounding a large language model that enables it to operate as an AI agent. It manages
> tool use, memory, state persistence, execution environments and feedback loops, as opposed
> to the model's internal reasoning."

Formalised as **`Agent = Model + Harness`**.

## Why a harness exists at all `[FACT]`

Two model properties generate the whole discipline:

1. **The model is stateless.** No memory between calls.
2. **The model only emits text.** It cannot open a file, run a test, or make a commit.

The consequence the article draws is the deepest idea in the field:

> "Rather than repeatedly re-reading an ever-growing transcript inside the context window,
> a harness can offload record-keeping into a structured software environment that manages
> the agent's state."

See [[Context Window as a Budget]] and [[Agent State]].

## The honest scope limit `[FACT]`

> "A minimal harness is unnecessary for a single prompt-and-response exchange, but becomes
> important as tasks grow multi-step, tool-oriented, or long-running."

See [[When Not to Build a Harness]]. Quote this at anyone building a harness for a one-shot
prompt.

## Prior art the term inherits `[FACT]`

The article is clear that the *mechanisms* predate the *word*:

- **ReAct** — model alternating between reasoning and acting in a loop (peer-reviewed).
- **Toolformer** — model calling external tools.
- The **UK AI Security Institute** described an AI agent as model **plus scaffolding** back
  in **2023** — three years before "harness engineering" was named.
- Adjacent senses of "harness": the **test harness** in software testing, **evaluation
  harnesses** in LLM benchmarking, and the **environment/wrapper** around a learning agent
  in reinforcement learning.

See [[Lineage of the Word Harness]].

## Contested attribution `[FACT — that it is contested]`

> "The vocabulary of 'harness engineering' emerged in **early 2026**, and attribution of the
> specific phrase is **contested**."

| Claimant | Contribution | Date |
|---|---|---|
| **Mitchell Hashimoto** (co-founder, HashiCorp) | Blog post describing engineering a permanent fix into the agent's environment each time it makes a mistake | Feb 2026 |
| **Vivek Trivedy** (LangChain) | *The Anatomy of an Agent Harness* — derives components from `Agent = Model + Harness` | Mar 2026 |
| **OpenAI** | Widely-cited engineering report on a large agent-built codebase | Feb 2026 |
| Thoughtworks, LangChain, Anthropic follow-on writing | Spread the term | 2026 |

> [!warning] Unverified
> I could **not** locate the Mitchell Hashimoto post at an obvious URL on `mitchellh.com`
> (404 on the path I tried). The attribution is reported here **as Wikipedia's claim**, not
> as a verified fact. Treat it as `[UNVERIFIED]` until you find the original.
> The Trivedy and OpenAI items **are** independently verified — see
> [[Source - Anatomy of an Agent Harness]] and [[Source - OpenAI Harness Engineering]].

## Böckeler's framework, as summarised by Wikipedia `[FACT]`

The article independently records the inner/outer and guides/sensors distinctions, which I
verified directly at the primary source — see
[[Source - Harness Engineering for Coding Agent Users]].

## Academic uptake `[UNVERIFIED]`

The article reports that by mid-2026 the harness had become an object of academic study,
naming two works:

- **Self-Harness** — an agent that iteratively mines its own failures to propose and validate
  changes to its own harness.
- **Harness-1** — an open-source search agent that improved retrieval accuracy chiefly by
  redesigning the software environment around the model rather than enlarging the model.

I have **not** verified either paper. Do not cite them from this note. If Harness-1's claim
holds up it is significant evidence for [[Harness Beats Model Choice]] — worth chasing.

## Position relative to prompt and context engineering `[FACT — as Wikipedia frames it]`

> "Harness engineering is often positioned as a broader layer than prompt engineering, which
> optimises a single interaction, or context engineering, which governs what information the
> model sees at a given moment; in this framing the harness designs the whole operational
> environment and contains the other two as parts."

And the property that distinguishes a harness from ordinary tooling:

> "A distinguishing feature is that the component being wrapped is **non-deterministic**, so
> a harness is designed to recover gracefully when the model fabricates an action or reports
> a task as finished when it is not."

That sentence is the cleanest one-line answer to *"how is harness engineering different from
developer tooling?"* — see [[Developer Tooling vs Harness Tooling]].

`[NOTE]` This containment framing **conflicts** with Böckeler's, who calls a coding-agent
harness "a specific form of context engineering." Documented in [[The Unified Mental Model]].

## Related

- [[Harness Engineering]] · [[Lineage of the Word Harness]] · [[When Not to Build a Harness]]
- [[Source - Harness Engineering for Coding Agent Users]] · [[Source - OpenAI Harness Engineering]]
- [[Sources MOC]]
