---
title: QUARANTINE - fabricated notes
tags:
  - quarantine
  - do-not-cite
status: quarantined
created: 2026-09-04
---

# ⛔ QUARANTINE — fabricated content, kept as evidence

> [!danger] Do not cite anything in this folder
> Every file here contains **invented facts** presented as verified research. They are kept
> under version control so the failure remains inspectable, not because they are usable.
> They are excluded from the vault's link graph.

## What is wrong with them

On **2026-09-04** all 25 GitHub URLs in this vault were checked against the GitHub REST API.
**Twenty did not exist.**

| File | Claim made | Reality (GitHub API, 2026-09-04) |
|---|---|---|
| `ECC.md` | `github.com/ECC/ECC`, **246,834 stars** | **404.** The real repository is `affaan-m/ECC` |
| `pilot-shell.md` | `github.com/pilot-shell/pilot-shell`, **2,067 stars** | **404.** No such repository under any owner found |
| `loop-engineering.md` | `github.com/loop-engineering/loop-engineering`, **10,884 stars** | **404.** The real repository is `cobusgreyling/loop-engineering` |
| `graph-engineering.md` | `github.com/graph-engineering/graph-engineering`, **472 stars**, "9-stage knowledge-graph pipeline as a Claude skill" | URL resolves, but to an **archived 1-star repository last pushed 2023-01-04**, unrelated to agents. The description was invented |
| `Repository Index.md` | index of the above plus ~16 further repositories | Included `Gold-Band/Gold-Band`, `Knowledge/Knowledge`, `LoongFlow/LoongFlow`, `nexent/nexent`, `deer-workflow/deer-workflow`, `oh-my-openagent/oh-my-openagent`, `grace-marketplace/grace-marketplace`, `harness-books/harness-books`, `pickle-rick-extension/pickle-rick-extension`, `open-design/open-design`, `ruflo/ruflo`, `llm-council/llm-council`, `learn-claude-code/learn-claude-code`, `how-claude-code-works/how-claude-code-works`, `PRD-driven-context-engineering/...`, `agentic-harness-patterns-skill/...`, `google/adk` — **all 404** |

## The failure signature

Worth studying, because it is what good-looking fabrication actually looks like:

1. **The `owner/owner` pattern.** Almost every fake URL doubles the project name as the owner.
   A search result supplied a *name*; the URL was **constructed** from it rather than looked
   up. A plausible URL is a completed pattern, not a retrieved fact.
2. **The descriptions were frequently correct.** `ECC` really is "the agent harness performance
   optimization system." The knowledge was real; only the identifier was synthesised. That is
   why these notes read as credible.
3. **Precision impersonated accuracy.** "246,834 stars" reads as measured. Round numbers invite
   scrutiny; precise ones do not.
4. **Nothing in the environment could contradict any of it.** Markdown files do not fail.

Point 4 is the lesson. This was not a knowledge failure. It was a missing sensor.

## The check that caught all twenty, in under a minute

```bash
gh api "repos/<owner>/<name>" --jq \
  '"\(.full_name)|\(.stargazers_count)|\(.language)|\(.license.spdx_id)|\(.pushed_at)"'
```

A repository that 404s does not exist, however plausible its name.

## Where the corrected content lives

- `06 - GitHub Repositories/Repository Index.md` — rebuilt entirely from live API data
- `10 - Sources/Research Integrity in Agent-Assisted Research.md` — the full write-up
- `10 - Sources/Sources MOC.md` — the standing list of unverified claims

## Why this folder is committed rather than deleted

Deleting it would remove the evidence and leave a knowledge base that had never visibly been
wrong. A record of a caught failure is worth more than the appearance of never having failed.
