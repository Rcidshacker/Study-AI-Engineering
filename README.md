# Study AI Engineering

**A researched knowledge base on harness engineering, loop engineering, and graph engineering
for AI coding agents** — built from primary sources that were fetched and read, not summarised
from memory.

Written as an [Obsidian](https://obsidian.md) vault. Readable as plain Markdown on GitHub.

> **82 notes · 0 broken links · every cited URL re-checked 2026-09-04**

---

## The one idea

> **`Agent = Model + Harness`**
>
> You cannot change the model. Everything you *can* change is the harness, the loops that run
> on it, and the graphs that coordinate those loops.

---

## Start here

| If you want… | Read |
|---|---|
| The conceptual frame, with the common diagram corrected | [The Unified Mental Model](01%20-%20Foundations/The%20Unified%20Mental%20Model.md) |
| The thing everything else depends on | [The Verification Gap](02%20-%20Harness%20Engineering/The%20Verification%20Gap.md) |
| To build something in ninety minutes | [Coding Agent Harness](07%20-%20Practical%20Examples/Coding%20Agent%20Harness.md) |
| A plan, with an exit test per level | [Learning Roadmap](10%20-%20Sources/Learning%20Roadmap.md) |
| To know what is actually verified | [Sources MOC](10%20-%20Sources/Sources%20MOC.md) |
| The full map | [AI Engineering MOC](00%20-%20MOC/AI%20Engineering%20MOC.md) |

---

## What this vault argues

The brief that started this research proposed harness, loop and graph as three sibling
disciplines. **They are not siblings — they are layers, and the stack is cumulative:**

```
GRAPH     the system      nodes, edges, shared state, routing
LOOP      the runtime     trigger, act, verify, persist, stop
HARNESS   the substrate   instructions · tools · environment · state · feedback
CONTEXT   the information what the model sees this turn
PROMPT    the instruction how this turn is phrased
```

Each layer keeps the ones below it. A loop cannot create a verification signal the harness does
not supply, and a graph cannot either. Full argument, including the one genuine unresolved
disagreement in the literature:
[The Unified Mental Model](01%20-%20Foundations/The%20Unified%20Mental%20Model.md).

---

## The six load-bearing claims

Confidence is stated, not implied. Each traces to a source note.

| Claim | Confidence |
|---|---|
| On long tasks, the environment explains more outcome variance than model choice | medium-high — no controlled study exists |
| Most agent unreliability reduces to the agent being unable to tell it was wrong | high |
| The thing that did the work must not decide the work is done | high |
| Anything the agent reads is a prompt, including error messages | high — two independent arrivals |
| Harness, loop and graph are layers, not alternatives | medium-high |
| Fix the class of failure, not the instance | high — three independent arrivals |

---

## Primary sources

Each fetched and read directly. Dates and quotes live in the source notes so there is one place
to correct each fact.

| Source | Author | Published |
|---|---|---|
| [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/) | Ryan Lopopolo, OpenAI | 2026-02-11 |
| [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) | Anthropic | 2025-11-26 |
| [Harness engineering for coding agent users](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) | Birgitta Böckeler, Thoughtworks | 2026-04-02 |
| [The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/) | Vivek Trivedy, LangChain | 2026-03-10 |
| [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) | Erik S. & Barry Zhang, Anthropic | 2024-12-19 |
| [Loop Engineering](https://addyosmani.com/blog/loop-engineering/) | Addy Osmani | mid-2026 |
| [Agent harness](https://en.wikipedia.org/wiki/Agent_harness) | Wikipedia | — |
| [Claude Code documentation](https://code.claude.com/docs) | Anthropic | continuous |

Repositories whose source was actually read:
[mini-swe-agent](https://github.com/SWE-agent/mini-swe-agent) ·
[ralph](https://github.com/snarktank/ralph) ·
[learn-harness-engineering](https://github.com/walkinglabs/learn-harness-engineering) ·
[defending-code-reference-harness](https://github.com/anthropics/defending-code-reference-harness) ·
[learn-claude-code](https://github.com/shareAI-lab/learn-claude-code)

---

## Structure

```
00 - MOC/                  maps of content — start here
01 - Foundations/          agents, loops, state, context
02 - Harness Engineering/  instructions · tools · environment · state · feedback
03 - Loop Engineering/     goals, verification, stopping conditions
04 - Graph Engineering/    nodes, edges, shared state, routing
05 - Claude Code/          verified against the official docs
06 - GitHub Repositories/  verified against the GitHub API
07 - Practical Examples/   reproducible configurations
08 - Comparisons/          how the terms relate
09 - Scenarios/            worked situations
10 - Sources/              evidence base, glossary, roadmap
```

Two folders are **not** part of the knowledge base:

- **`_QUARANTINE_fabricated_2026-09-04/`** — five notes containing invented repositories, kept
  as evidence. [Read its README first.](_QUARANTINE_fabricated_2026-09-04/README.md)
- `_ARCHIVE_unverified_2026-09-03/` — superseded first-pass drafts.

---

## Evidence standard

Every claim carries a tag:

| Tag | Meaning |
|---|---|
| `[FACT]` | stated in a primary source I read directly |
| `[PRACTICE]` | widely-reported community practice |
| `[OPINION]` | a named person's position, attributed |
| `[INFERENCE]` | synthesis — reasoning, not reporting |
| `[UNVERIFIED]` | reported somewhere, could not confirm |
| `[CAUTION]` | verified to exist, with a reliability or bias caveat |

Three rules the vault follows:

1. **Claims live in source notes; concept notes cite source notes.** One place to correct each
   fact, and no concept note is the sole home of a claim.
2. **Every source note carries a `verified:` date.** Treat anything months old as stale.
3. **Unverifiable claims are marked, not dropped.** The standing list is in
   [Sources MOC](10%20-%20Sources/Sources%20MOC.md).

---

## Why there is a quarantine folder

An earlier pass of this research produced **twenty fabricated GitHub repositories** with
invented star counts, presented as verified fact. Every URL was plausible. Most descriptions
were correct. Only the identifiers were invented — `ECC/ECC` instead of `affaan-m/ECC`,
`loop-engineering/loop-engineering` instead of `cobusgreyling/loop-engineering`.

They were caught by one command:

```bash
gh api "repos/<owner>/<name>" --jq '"\(.full_name)|\(.stargazers_count)|\(.pushed_at)"'
```

Under a minute, against every URL in the vault. An hour of confident writing; a minute of
checking. That asymmetry is the general case.

The fabricated notes are **kept, committed and documented** rather than deleted. A record of a
caught failure is worth more than the appearance of never having failed — and it is this
vault's own worked example of the subject it is about.
Write-up: [Research Integrity in Agent-Assisted Research](10%20-%20Sources/Research%20Integrity%20in%20Agent-Assisted%20Research.md).

---

## Worked scenarios

Each takes a failure people actually hit and designs the cheapest control that removes it.

| Scenario | Notable finding |
|---|---|
| [Coding Agent Harness](07%20-%20Practical%20Examples/Coding%20Agent%20Harness.md) | full five-subsystem coverage in ~90 minutes |
| [The Verification Gap](02%20-%20Harness%20Engineering/The%20Verification%20Gap.md) | weak verification is more dangerous than none — it manufactures confidence |
| [Autonomous Test Fixer](09%20-%20Scenarios/Autonomous%20Test%20Fixer.md) | the best first loop, because the judge already exists |
| [Skill Routing](09%20-%20Scenarios/Skill%20Routing.md) | at 20+ skills the router is fine; the **description budget** (1% of context) is what breaks |
| [Multi Agent Coding System](09%20-%20Scenarios/Multi%20Agent%20Coding%20System.md) | of five proposed roles, two survive scrutiny |
| [Research Agent](09%20-%20Scenarios/Research%20Agent.md) | markdown files do not fail, so the sensors must be built |
| [Production Coding Agent](09%20-%20Scenarios/Production%20Coding%20Agent.md) | a readiness checklist, from the only detailed public account |

---

## Using it

**In Obsidian** — clone and open the folder as a vault. Wikilinks resolve, and the graph view
is the point.

```bash
git clone https://github.com/Rcidshacker/Study-AI-Engineering.git
```

**On GitHub** — start from
[AI Engineering MOC](00%20-%20MOC/AI%20Engineering%20MOC.md). Wikilinks inside notes render as
plain text here; the links in this README are real.

---

## Scope and honesty

This is one person's research vault, not a textbook. Specifically:

- **The field is about a year old.** "Harness engineering" was named in early 2026 with
  contested attribution; "loop engineering" mid-2026; "graph engineering" around July 2026,
  [from a joke](04%20-%20Graph%20Engineering/Graph%20Engineering%20Origin%20and%20Fact-Check.md).
- **There are no controlled studies.** The strongest evidence is detailed first-party
  engineering reports. Effect sizes are illustrative.
- **Product surfaces move weekly.** Claude Code capability claims were verified on 2026-09-04
  and should be re-checked against the docs before you build on a command name.
- **Unverified claims are listed**, not hidden, in
  [Sources MOC](10%20-%20Sources/Sources%20MOC.md).

If you find something wrong, that is the point of the evidence tags — open an issue.

---

## Licence

Notes and prose in this repository are the author's own. Quoted material belongs to the cited
sources and is used for commentary and study, with attribution and links throughout.
