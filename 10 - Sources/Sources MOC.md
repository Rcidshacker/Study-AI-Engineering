---
title: Sources MOC
aliases:
  - Source index
  - Source database
tags:
  - moc
  - sources
status: evergreen
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Sources MOC

The evidence base for this vault. **Every entry below was fetched and read on the date in its
"verified" column.** Anything I could not verify is listed in the last section rather than
quietly omitted.

---

## Evidence legend

Used throughout the vault. Check the tag before you rely on a claim.

| Tag | Meaning |
|---|---|
| `[FACT]` | Stated in a primary source I read directly |
| `[PRACTICE]` | Widely-reported community practice, no single authority |
| `[OPINION]` | A named person's position, attributed |
| `[INFERENCE]` | My own synthesis — reasoning, not reporting |
| `[UNVERIFIED]` | Reported somewhere, but I could not confirm it |
| `[CAUTION]` | Verified to exist, but with a reliability or bias caveat |

---

## Tier 1 — primary sources, first-party

Written by the people who did the work.

| Source | Author / publisher | Published | Verified | Why it matters |
|---|---|---|---|---|
| [[Source - OpenAI Harness Engineering]] | Ryan Lopopolo, OpenAI | 2026-02-11 | 2026-09-04 | The origin point for *harness engineering* as a discipline name. ~1M lines, ~1,500 PRs, 0 hand-written lines |
| [[Source - Anthropic Effective Harnesses for Long-Running Agents]] | Anthropic | 2025-11-26 | 2026-09-04 | Earlier than OpenAI's. Feature lists, initializer/coding split, "compaction isn't sufficient" |
| [[Source - Anthropic Building Effective Agents]] | Anthropic | 2024-12 | 2026-09-04 (existence) | The five workflow patterns that later get called graphs |
| Claude Code documentation | Anthropic | continuously | 2026-09-04 | Ground truth for every [[Claude Code]] capability claim in this vault |

## Tier 2 — primary sources, practitioner

| Source | Author / publisher | Published | Verified | Why it matters |
|---|---|---|---|---|
| [[Source - Harness Engineering for Coding Agent Users]] | Birgitta Böckeler, Thoughtworks | 2026-04-02 | 2026-09-04 | **The best framework**: guides/sensors × computational/inferential; inner vs outer harness |
| [[Source - Anatomy of an Agent Harness]] | Vivek Trivedy, LangChain | 2026-03-10 | 2026-09-04 | `Agent = Model + Harness`; derivation method; filesystem as foundational primitive. `[CAUTION]` vendor |
| [[Source - Addy Osmani Loop Engineering]] | Addy Osmani | mid-2026 | 2026-09-04 | Named *loop engineering*. `[CAUTION]` exact date of the original post unconfirmed |

## Tier 3 — secondary and tertiary

| Source | Type | Verified | Why it matters |
|---|---|---|---|
| [[Source - Learn Harness Engineering Course]] | open-source course, 14,741★ | 2026-09-04 | The only source treating harness + loop + graph as one curriculum; ships templates; fact-checks the graph hype |
| [[Source - Wikipedia Agent Harness]] | encyclopedia | 2026-09-04 | Neutral account of **contested attribution**; the ReAct/Toolformer/AISI lineage |

## Repositories

Verified via the GitHub API on 2026-09-04 — see [[Repository Index]] for the full table and
the verification command.

- [[GitHub - SWE-agent mini-swe-agent]] — the ~100-line agent loop, read in full
- [[GitHub - snarktank ralph]] — the canonical autonomous loop, read in full
- [[GitHub - walkinglabs learn-harness-engineering]] — the course repo
- [[GitHub - anthropics defending-code-reference-harness]] — Anthropic's own reference harness

---

## How the source layer is meant to be used

1. **Concept notes cite source notes; source notes cite URLs.** No concept note should be the
   only place a claim exists.
2. **Dates and numbers live in source notes**, so there is one place to correct them.
3. **Re-verification is expected.** Star counts, product commands and doc URLs all move. Each
   source note carries a `verified:` date; treat anything older than a few months as stale.

---

## Known gaps and unverified claims

Kept deliberately visible.

| Claim | Where it comes from | Status |
|---|---|---|
| Mitchell Hashimoto's Feb 2026 post coined "harness engineering" | Wikipedia | `[UNVERIFIED]` — not found at the obvious URL |
| *Loop Engineering* published 7 June 2026 | the course | `[UNVERIFIED]` — the live page shows "Aug 2026"; the RSS feed confirms only *Own the Outer Loop* (15 Jul 2026) and *Practical Loop Engineering* (14 Aug 2026) |
| "Self-Harness" and "Harness-1" research papers | Wikipedia | `[UNVERIFIED]` — not located. If Harness-1 holds up it is strong evidence for [[Harness Beats Model Choice]] |
| The 20% → 100% staged case study | the course | `[UNVERIFIED]` — no team, publication or dataset named; n=5 per stage |
| Boris Cherny's autonomy figures (259 PRs, >80% of production code, 76% success rate) | the course, citing a podcast | `[UNVERIFIED]` — I did not reach the primary recording |
| "+18% accuracy, −85% cost" for graph engineering | widely circulated | **Actively debunked** — see [[Graph Engineering Origin and Fact-Check]] |

> [!danger] This vault's own failure
> An earlier pass of this research produced **20 fabricated GitHub repositories** with
> invented star counts, presented as fact. They were caught by checking every URL against the
> GitHub API, and are quarantined in `_QUARANTINE_fabricated_2026-09-04/` rather than deleted.
> The incident is the vault's own worked example of [[The Verification Gap]] and is written up
> in [[Research Integrity in Agent-Assisted Research]]. Treat it as the reason every table
> above carries a verification date.

---

## Related

- [[AI Engineering MOC]] · [[Harness Loop Graph MOC]] · [[Repository Index]]
- [[Research Integrity in Agent-Assisted Research]]
