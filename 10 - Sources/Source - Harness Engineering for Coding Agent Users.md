---
title: Source - Harness Engineering for Coding Agent Users
aliases:
  - Böckeler harness engineering
  - Bockeler 2026
  - Guides and Sensors
tags:
  - source
  - primary-source
  - harness-engineering
  - thoughtworks
source-type: engineering-article
author: Birgitta Böckeler (Distinguished Engineer, Thoughtworks)
publisher: martinfowler.com
published: 2026-04-02
url: https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html
reliability: high
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Source - Harness Engineering for Coding Agent Users

> [!info] Verification
> Fetched and read directly on 2026-09-04. HTTP 200. Published **02 April 2026** on
> martinfowler.com by Birgitta Böckeler. The article says it "updates an earlier memo
> outlining my first impressions of harness engineering."

## Why this is the most useful source for *you*

[[Source - OpenAI Harness Engineering]] describes what a frontier lab did with a bespoke
setup. Böckeler's article is scoped explicitly to **users of coding agents** — people who
did not build the agent and are assembling controls on top of one. That is your position.
It also supplies the single best organising framework in the literature.

## The inner/outer distinction `[FACT]`

> "The term *harness* has emerged as a shorthand to mean everything in an AI agent except
> the model itself — `Agent = Model + Harness`. That is a very wide definition, and
> therefore worth narrowing down."

- **Inner harness** — what the agent vendor ships: system prompt, code retrieval mechanism,
  built-in orchestration. You don't control it.
- **Outer harness** — what *you* assemble for your use case and system: instruction files,
  MCP servers, skills, hooks, tests, linters.

She acknowledges the metaphor strains ("Have you ever tried to put a harness on the inside
of a dog?") and accepts it anyway for navigational value. See [[Inner Harness vs Outer Harness]].

**A well-built outer harness has two goals**: raise the probability the agent gets it right
first time, *and* provide a feedback loop that self-corrects issues before they reach human
eyes — reducing review toil and wasted tokens.

## The core framework: Guides × Sensors, Computational × Inferential `[FACT]`

This 2×2 is the most reusable idea in the whole harness literature.

**By direction:**

- **Guides (feedforward controls)** — anticipate behaviour and steer *before* the agent acts.
- **Sensors (feedback controls)** — observe *after* the agent acts and let it self-correct.

> "Separately, you get either an agent that keeps repeating the same mistakes
> (feedback-only) or an agent that encodes rules but never finds out whether they worked
> (feed-forward-only)."

**By execution type:**

- **Computational** — deterministic, CPU, milliseconds to seconds, reliable. Tests, linters,
  type checkers, structural analysis.
- **Inferential** — semantic, GPU/NPU, slower and more expensive, non-deterministic. AI code
  review, LLM-as-judge.

Her published examples:

| Control | Direction | Type | Implementation |
|---|---|---|---|
| Coding conventions | feedforward | inferential | `AGENTS.md`, Skills |
| How to bootstrap a project | feedforward | both | Skill + bootstrap script |
| Code mods | feedforward | computational | Tool with OpenRewrite recipes |
| Structural tests | feedback | computational | Pre-commit / agent hook running ArchUnit boundary checks |
| How to review | feedback | inferential | Skills |

See [[Guides and Sensors]] for the working note and the Claude Code mapping.

## "A positive kind of prompt injection" `[FACT]`

Sensors are "particularly powerful when they produce signals that are optimised for LLM
consumption, e.g. custom linter messages that include instructions for the self-correction."

> [!note] Independent convergence
> [[Source - OpenAI Harness Engineering]] reports exactly the same practice — "because the
> lints are custom, we write the error messages to inject remediation instructions into
> agent context" — arrived at independently, two months earlier, at a different company.
> Two independent arrivals is the strongest signal in this vault that a technique is real
> rather than fashionable. See [[Feedback Quality]].

## The steering loop `[FACT]`

> "The human's job in this is to steer the agent by iterating on the harness. Whenever an
> issue happens multiple times, the feedforward and feedback controls should be improved to
> make the issue less probable to occur in the future, or even prevent it."

This is the same reflex as OpenAI's "what capability is missing?" and Mitchell Hashimoto's
reported practice of engineering a permanent environment fix after each mistake. See
[[Fix the Class Not the Instance]].

She adds that agents themselves make this cheap: they "can help write structural tests,
generate draft rules from observed patterns, scaffold custom linters, or create how-to
guides from codebase archaeology."

## Timing: keep quality left `[FACT]`

Distribute controls across the change lifecycle by cost, speed and criticality:

- **Pre-commit / pre-integration** — linters, fast test suites, a basic code-review agent.
- **Post-integration pipeline** — mutation testing, broader review that sees the bigger picture.
- **Continuous drift sensors, outside the change lifecycle** — dead-code detection, test-coverage
  quality analysis, dependency scanners.
- **Runtime feedback** — agents watching degrading SLOs, AI judges sampling response quality
  and flagging log anomalies.

The last two categories are the ones most harnesses omit. See [[Continuous Drift Sensors]].

## Three harness categories `[FACT]`

She argues "harness" is too generic a word and qualifies it:

1. **Maintainability harness** — internal code quality. Easiest today, because decades of
   existing tooling apply. Computational sensors reliably catch duplicate code, cyclomatic
   complexity, missing coverage, architectural drift, style.
2. **Architecture fitness harness** — conformance to intended architecture.
3. **Behaviour harness** — does the software actually do the right thing.

The harness "acts like a cybernetic governor, combining feed-forward and feedback to
regulate the codebase towards its desired state."

## Harnessability `[FACT]`

A property of the *codebase*, not the agent: how amenable your system is to being regulated
by a harness at all. A codebase with no tests, no types and no module boundaries offers
almost nothing for computational sensors to attach to. See [[Harnessability]].

## Relationship to context engineering `[FACT]`

Her sidebar answers the user's own question directly:

> "Context engineering provides us with the means to make guides and sensors available to
> the agent. Engineering a user harness for a coding agent is **a specific form of context
> engineering**."

`[NOTE]` This makes containment *bidirectional* in the literature: Wikipedia frames the
harness as containing context engineering; Böckeler frames the coding-agent harness as a
form of context engineering. The disagreement is real and worth keeping. See
[[The Unified Mental Model]] and [[Prompt Engineering vs Context Engineering]].

## Related

- [[Harness Engineering]] · [[Guides and Sensors]] · [[Inner Harness vs Outer Harness]] · [[Harnessability]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Wikipedia Agent Harness]]
- [[Sources MOC]]
