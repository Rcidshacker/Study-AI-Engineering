---
title: Repository Index
aliases:
  - GitHub Repository Index
  - GitHub Repository MOC
tags:
  - moc
  - github
  - index
status: evergreen
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Repository Index

> [!danger] Read this first — why this file was rewritten
> The previous version of this index, and four repository notes beside it, contained
> **fabricated repositories**: invented owner slugs (`ECC/ECC`, `pilot-shell/pilot-shell`,
> `loop-engineering/loop-engineering`) and invented star counts (e.g. "246,834" attributed to
> a repo that does not exist at that path). I checked all 25 GitHub URLs in the vault against
> the GitHub API on 2026-09-04: **20 were fake.** They are quarantined in
> `_QUARANTINE_fabricated_2026-09-04/` rather than deleted, so the failure stays inspectable.
>
> **Every row below was pulled live from the GitHub REST API on 2026-09-04.** Star counts
> move; re-verify before quoting. The lesson is recorded in
> [[Research Integrity in Agent-Assisted Research]] — a research harness that does not
> verify its own outputs produces exactly this failure, which is the vault's own worked
> example of [[The Verification Gap]].

---

## How to read this index

Ranked by **what you learn from reading the source**, not by stars. A 6,900-star repo you can
read in an afternoon beats a 200,000-star one you cannot.

Legend: **★** = stars · **Read** = how much of it is worth reading · verified 2026-09-04.

---

## Tier 1 — read the source

These reward actually opening the code.

| Repo | ★ | Language | Why | Read |
|---|---:|---|---|---|
| [[GitHub - SWE-agent mini-swe-agent]] | 6,938 | Python · MIT | ~100-line agent that scores >74% on SWE-bench Verified. The clearest existing answer to "what is minimally in an agent loop." | all of it |
| [[GitHub - snarktank ralph]] | 21,700 | TypeScript · MIT | The canonical autonomous loop. `ralph.sh` is ~120 lines and contains a complete stopping-condition design. | `ralph.sh`, `CLAUDE.md`, `prd.json.example` |
| [[GitHub - anthropics defending-code-reference-harness]] | 7,403 | Python · NOASSERTION | Anthropic's own reference harness: skills + an autonomous recon→find→verify→report→patch pipeline. A first-party worked example. | `CLAUDE.md`, `.claude/`, `harness/` |
| [[GitHub - walkinglabs learn-harness-engineering]] | 14,741 | TypeScript · MIT | 14 lectures covering harness, loop *and* graph in one framework, with templates. The vault's anchor secondary source. | `docs/en/lectures/02, 13, 14`, `docs/en/resources/templates/` |
| [[GitHub - shareAI-lab learn-claude-code]] | 75,990 | Python · MIT | "Bash is all you need" — a nano Claude-Code-like agent harness built from 0 to 1. | the build-up commits |

## Tier 2 — read the docs, skim the source

| Repo | ★ | Language | Why |
|---|---:|---|---|
| `anthropics/claude-code` | 143,935 | Python · no licence file | The tool itself. The repo is primarily issues + docs; the ground truth for [[Claude Code]] capability claims. |
| `Windy3f3f3f3f/how-claude-code-works` | 3,578 | MIT | Community reverse-engineering of Claude Code internals: architecture, agent loop, context engineering. `[CAUTION]` third-party inference, not official. |
| `x1xhlol/system-prompts-and-models-of-ai-tools` | 143,318 | GPL-3.0 | Collected system prompts of many coding tools. The best available window onto **inner harness** design. `[CAUTION]` provenance is unverifiable per-file; treat as indicative. |
| `openai/codex` | 121,228 | Rust · Apache-2.0 | The other major terminal coding agent. Useful contrast for [[Claude Code vs Agent Frameworks]]. |
| `SWE-bench/SWE-bench` | 5,769 | Python · MIT | The benchmark that made coding agents measurable. Read the harness design, not just the leaderboard. See [[Agent Evaluation]]. |
| `EleutherAI/lm-evaluation-harness` | 13,881 | Python · MIT | Where the word *harness* comes from in ML. Created **2020-08-28** — five years before "harness engineering." See [[Lineage of the Word Harness]]. |
| `langchain-ai/langgraph` | 41,000 | Python · MIT | The reference implementation of the [[Graph Engineering]] node/edge/state/routing model. Read it even if you never use it. |
| `oraios/serena` | 28,783 | Python · MIT | Semantic retrieval and editing over MCP — a concrete answer to "what should a code tool expose to an agent?" |

## Tier 3 — context and comparison

| Repo | ★ | Note |
|---|---:|---|
| `affaan-m/ECC` | 246,990 | "The agent harness performance optimization system." Very large star count; correspondingly large surface. `[CAUTION]` treat scale as popularity, not as evidence of quality. |
| `langchain-ai/deepagents` | 28,893 | "The batteries-included agent harness." Useful as a **component checklist** for [[Harness Components]]. |
| `earendil-works/pi` | 101,417 | "unified LLM API, agent loop, TUI, coding agent CLI" — another readable agent-loop implementation. |
| `cobusgreyling/loop-engineering` | 10,895 | Created **2026-06-09**, days after the term was named — corroborating evidence for the June-2026 timeline in [[Source - Addy Osmani Loop Engineering]]. |
| `HKUDS/DeepCode` | 16,482 | Describes itself as "Agent Harness & Loop Engineering & Multi-Agent Orchestration" — a rare single repo naming all three. |
| `addyosmani/agent-skills` | 91,975 | "Production-grade engineering skills for AI coding agents," from the person who named loop engineering. |
| `anomalyco/opencode` | 203,516 | Open-source coding agent; a third inner-harness design to compare. |

---

## Terminology collision, recorded `[FACT]`

`harness/harness` (38,228★) is **Harness Open Source**, a CI/CD and developer platform. It has
nothing to do with agent harnesses. Likewise `browser-use/browser-harness` uses the word in a
different sense again. If you search GitHub for "harness," expect three unrelated meanings.
See [[Lineage of the Word Harness]].

---

## What I looked for and did not find `[FACT]`

Honest gaps, so the index is not read as complete:

- **No repository named `pilot-shell`** exists at the URL previously cited here (404).
- **`graph-engineering/graph-engineering` does exist** but is an archived, 1-star repository
  last pushed **2023-01-04**, unrelated to agents. The previous note's description of it as a
  "9-stage knowledge-graph pipeline Claude skill" was fabricated.
- **`google/adk`** 404s; the real Google Agent Development Kit lives at a different path.
  Verify before citing.
- I found **no widely-adopted repository** that implements "graph engineering" as a named
  discipline for coding agents. The graph work that exists is either general orchestration
  (LangGraph) or knowledge-graph indexing of codebases — two different things sharing a word.
  See [[Graph Engineering Origin and Fact-Check]].

---

## Verification method

```bash
gh api "repos/<owner>/<name>" --jq \
  '"\(.full_name)|\(.stargazers_count)|\(.language)|\(.license.spdx_id)|\(.pushed_at)"'
```

Run this before adding any repository to this vault. A repository that 404s does not exist,
however plausible its name.

---

## Related

- [[Harness Engineering]] · [[Loop Engineering]] · [[Graph Engineering]]
- [[Research Integrity in Agent-Assisted Research]] · [[Sources MOC]]
