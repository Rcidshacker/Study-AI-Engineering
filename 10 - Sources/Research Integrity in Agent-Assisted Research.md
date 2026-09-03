---
title: Research Integrity in Agent-Assisted Research
aliases:
  - The fabrication incident
  - Verifying agent research output
tags:
  - research-method
  - verification
  - harness-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Research Integrity in Agent-Assisted Research

> [!abstract] One line
> This vault fabricated twenty GitHub repositories before anyone noticed. The incident is kept
> here because it is a cleaner demonstration of [[The Verification Gap]] than any example I
> could have invented.

---

## What happened `[FACT]`

An earlier research pass on this vault produced a `06 - GitHub Repositories/` folder and a
`Repository Index.md` containing repositories such as:

| Cited | Claimed stars | Reality (GitHub API, 2026-09-04) |
|---|---|---|
| `github.com/ECC/ECC` | 246,834 | **404.** The real repo is `affaan-m/ECC` |
| `github.com/pilot-shell/pilot-shell` | 2,067 | **404.** No such repository |
| `github.com/loop-engineering/loop-engineering` | 10,884 | **404.** The real repo is `cobusgreyling/loop-engineering` |
| `github.com/graph-engineering/graph-engineering` | 472 | **Exists**, but is an archived 1-star repo last pushed 2023-01-04, unrelated to agents |

Of **25 GitHub URLs** in the vault, **20 did not exist**. The star counts were invented to
four and five significant figures.

---

## The failure signature, and why it is worth studying

`[INFERENCE]` Note the shape of the errors, because it is diagnostic:

1. **The `owner/owner` pattern.** Almost every fake URL doubles the project name as the owner
   (`ECC/ECC`, `pilot-shell/pilot-shell`). A search result gave a *name*; the URL was
   *constructed* from it rather than looked up. **A plausible-looking URL is a completed
   pattern, not a retrieved fact.**
2. **The descriptions were often right.** `ECC` really is "the agent harness performance
   optimization system." The *knowledge* was real; only the *identifier* was synthesised. This
   is why the notes read as credible — they were credible, apart from being uncheckable.
3. **Precision impersonated accuracy.** "246,834 stars" reads as measured. Round numbers would
   have invited scrutiny; precise ones did not.
4. **Nothing in the environment could contradict any of it.** Markdown files do not fail.

Point 4 is the whole lesson. This was not a knowledge failure — it was a **harness failure**.
There was no sensor. See [[Guides and Sensors]].

---

## The fix, which is one command

```bash
gh api "repos/<owner>/<name>" --jq \
  '"\(.full_name)|\(.stargazers_count)|\(.language)|\(.license.spdx_id)|\(.pushed_at)"'
```

A repository that 404s does not exist, however plausible its name. Running this over every URL
in the vault took under a minute and caught all twenty.

`[INFERENCE]` That asymmetry — an hour of confident writing, a minute of verification — is
the general case. **The cheap check almost always exists; what is missing is the habit of
running it, and the harness that runs it for you.**

---

## The research harness this vault now uses

| Claim type | Verification | Where recorded |
|---|---|---|
| A URL exists | fetch it, record HTTP status + `<title>` | source note frontmatter |
| A repo's stats | GitHub API, with a `verified:` date | [[Repository Index]] |
| A quote | read the source directly; quote verbatim | source note |
| A date | prefer a machine-readable feed over rendered page text | source note |
| A feature exists | official documentation, not community write-ups | note header |
| A number | trace to a primary source or mark `[UNVERIFIED]` | inline |

Plus one structural rule: **claims live in source notes, concept notes cite source notes.**
One place to correct each fact, and a concept note can never be the sole home of a claim.

---

## The guides and sensors, in the vault's own terms

| Control | Type | Implementation |
|---|---|---|
| Evidence legend on every claim | **guide**, inferential | `[FACT]`/`[INFERENCE]`/`[UNVERIFIED]` tags |
| "No URL without a fetch" | **guide**, procedural | this note |
| URL liveness check | **sensor**, computational | `gh api` / HTTP status sweep |
| Broken-wikilink scan | **sensor**, computational | link-target vs. filename diff |
| Known-gaps table | **sensor**, inferential | [[Sources MOC]] |

`[INFERENCE]` The computational sensors are the only ones that would have caught this
incident. The legend and the procedure are guides — necessary, and insufficient on their own,
exactly as [[Guides and Sensors]] predicts.

---

## Why the fabricated notes were quarantined, not deleted

They sit in `_QUARANTINE_fabricated_2026-09-04/`, excluded from the vault's link graph. `[INFERENCE]`
Deleting them would have removed the evidence and left a knowledge base that had never
visibly been wrong. A record of a caught failure is worth more than the appearance of never
having failed — and it is a standing reminder of what unverified agent output looks like when
it is *good*: fluent, specific, well-organised, and false.

---

## The transferable rule

> **Any claim an agent produces that could be checked by a command, and was not, should be
> treated as unverified — regardless of how confident, specific, or well-written it is.**

That applies to this vault, to your codebase, and to every note in here that carries a
`[FACT]` tag. The tags mean I checked. They do not mean you should not.

---

## Related

- [[The Verification Gap]] · [[False Completion]] · [[Guides and Sensors]] · [[Feedback Quality]]
- [[Research Agent]] · [[Sources MOC]] · [[Repository Index]]
