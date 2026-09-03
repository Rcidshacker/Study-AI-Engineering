---
title: Research Agent
aliases:
  - Scenario 5
  - Research loop
  - Knowledge base agent
tags:
  - scenario
  - loop-engineering
  - verification
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Scenario — Research Agent

> [!abstract] The task
> An agent researches a technical subject and produces a connected knowledge base. Design the
> harness and the research loop.

> [!danger] This vault is the worked example — including the failure
> The first attempt at this exact task produced **twenty fabricated GitHub repositories** with
> invented star counts, presented as verified fact. The design below is what the failure
> taught. Full account: [[Research Integrity in Agent-Assisted Research]].

---

## Why research is harder than coding

`[INFERENCE]` Coding has a natural oracle. Research does not, and the difference is decisive:

| | Coding | Research |
|---|---|---|
| Verification | tests pass or fail | ? |
| Wrong output looks like | a red build | **a well-written note** |
| Cost of an error | caught next run | propagates into every citing note |
| Failure is | loud | **silent** |

**Markdown files do not fail.** That single fact is why a research harness needs deliberately
constructed sensors — there is no build to break. See [[The Verification Gap]].

---

## The harness

### Feedback — build this first

`[INFERENCE]` A research harness has no sensors unless you write them. Three that pay for
themselves in minutes:

| Sensor | Type | Catches |
|---|---|---|
| **URL liveness sweep** | computational | fabricated sources. `gh api repos/<o>/<r>` or an HTTP status check |
| **Broken wikilink scan** | computational | dead ends, duplicate concepts under different names |
| **Unverified-claims register** | inferential | claims nobody chased down |

The first would have caught all twenty fabrications, in under a minute, at any point.

### Instructions — the evidence contract

Every claim carries a tag, and the tags are load-bearing rather than decorative:

```text
[FACT]        stated in a primary source I read directly
[PRACTICE]    widely-reported community practice
[OPINION]     a named person's position, attributed
[INFERENCE]   my own synthesis — reasoning, not reporting
[UNVERIFIED]  reported somewhere, could not confirm
[CAUTION]     real, but with a reliability or bias caveat
```

Plus two structural rules:

1. **Claims live in source notes; concept notes cite source notes.** One place to correct each
   fact, and no concept note is the sole home of a claim.
2. **No URL without a fetch.** A plausible URL is a completed pattern, not a retrieved fact.

### State

`sources.md` as a register with a `verified:` date per entry; the notes themselves as output;
git as the audit trail. `[INFERENCE]` The `verified:` date is what makes staleness visible —
without it, a two-year-old star count reads exactly like a fresh one.

### Environment and tools

Fetching, search, and — critically — **a way to read the actual artefact**: clone the repo,
fetch the API, read the paper. `[INFERENCE]` Reading *about* a repository is how the
fabrication happened; reading the repository is how it was caught.

---

## The loop

```text
  ┌──────────────────────────────────────────────────────────────┐
  │ 1. DISCOVER    find candidate sources                        │
  │ 2. VERIFY      fetch each one. 404 ⇒ it does not exist.       │  ← the gate
  │                Record status, title, date                    │
  │ 3. READ        the source itself, not a summary of it         │
  │ 4. EXTRACT     claims, with quotes and tags                   │
  │ 5. WRITE       a source note; then concept notes citing it    │
  │ 6. LINK        only where the connection carries meaning      │
  │ 7. SWEEP       URL liveness + broken links + unverified list  │
  │ 8. STOP        questions answered AND every source verified   │
  └──────────────────────────────────────────────────────────────┘
```

**Step 2 is a gate, not a step.** `[FACT]` Anthropic's prompt-chaining pattern names exactly
this device — "programmatic checks (see *gate*) on any intermediate steps to ensure that the
process is still on track."

**Step 3 is where quality is decided.** `[INFERENCE]` Search snippets give you the shape of a
claim without its substance — which is precisely how a note ends up with a correct description
attached to an invented identifier.

---

## Stopping conditions

| State | Detected by |
|---|---|
| Success | research questions answered **and** every cited URL verified **and** zero broken links |
| Impossible | the question needs a source that does not exist — record it as a gap |
| Budget | source count, time, or cost |
| **Stuck** | new sources stop changing conclusions — diminishing returns |

`[INFERENCE]` "Every cited URL verified" belongs in the success condition, not in a review
step. If verification is optional it will be skipped exactly when the writing is going well.

---

## Structure that keeps the graph honest

- **Atomic concept notes**; one idea each.
- **Source notes** carrying the quotes, dates and `verified:` stamps.
- **MOCs** that link only to notes that exist. `[INFERENCE]` A MOC linking to unwritten notes
  is a fabrication of a different kind — it asserts coverage the vault does not have.
- **Link only where the connection means something.** Decorative links dilute the graph the
  same way surplus rules dilute an instruction file.

---

## The transferable rule

> **Any claim an agent produces that could be checked by a command, and was not, is unverified —
> however confident, specific, or well-written it is.**

`[INFERENCE]` The asymmetry is the whole lesson: an hour of fluent writing, a minute of
checking. The cheap check almost always exists. What is missing is the habit, and the harness
that runs it for you.

---

## Related

- [[Research Integrity in Agent-Assisted Research]] · [[The Verification Gap]] · [[Guides and Sensors]]
- [[Stopping Conditions]] · [[Feedback Quality]] · [[Sources MOC]] · [[Scenarios MOC]]
