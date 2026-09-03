---
title: Comparisons MOC
aliases:
  - Comparisons index
tags:
  - moc
  - comparison
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# Comparisons MOC

Notes that exist to separate things people conflate. Each one answers a question you will be
asked, or will ask yourself.

---

## The notes

| Question | Note |
|---|---|
| Are harness, loop and graph alternatives? | [[Harness vs Loop vs Graph]] — no; they are layers |
| Did context engineering replace prompt engineering? | [[Prompt Engineering vs Context Engineering]] — no; it stacked on top |
| Isn't this all just good developer tooling? | [[Developer Tooling vs Harness Tooling]] — largely, minus the recovery layer |
| Isn't graph engineering just workflow orchestration? | [[Graph vs Workflow]] — same skeleton, different nodes |
| What do I control, and what does the vendor? | [[Inner Harness vs Outer Harness]] |
| Is the loop the agent's, or mine? | [[Inner Loops and Outer Loops]] |
| Does a better model remove the need for this? | [[Harness Beats Model Choice]] |
| Where does the whole thing fit together? | [[The Unified Mental Model]] |

---

## The three conflations worth being precise about

`[INFERENCE]`

**1. Layers read as alternatives.** The commonest error, and the most expensive: it produces
graph architectures for problems that a test command would have solved. Each layer can only be
as good as the one beneath it.

**2. Naming read as newness.** *Harness engineering* is roughly a year old, *loop engineering*
about a year, *graph engineering* two months and born from a joke. The **practices** are older
in every case — ReAct, Toolformer, workflow orchestration, feedback control. See
[[Lineage of the Word Harness]] and [[Graph Engineering Origin and Fact-Check]].

**3. Emphasis read as disagreement.** OpenAI writes about throughput and repository legibility;
Anthropic about continuity across sessions; Thoughtworks about controls for people who did not
build the agent. They agree on the substrate and differ on which part of it they needed most.

---

## The one genuine disagreement `[FACT]`

Where the harness sits relative to context engineering is unresolved:

| Claim | Source |
|---|---|
| The harness **contains** context engineering | [[Source - Wikipedia Agent Harness]] |
| A coding-agent harness **is a form of** context engineering | [[Source - Harness Engineering for Coding Agent Users]] |

This vault's resolution, marked as inference rather than fact: context engineering is the
**delivery mechanism**; the harness is larger than what it delivers. See
[[The Unified Mental Model]].

---

## Related

- [[AI Engineering MOC]] · [[Harness Loop Graph MOC]] · [[Glossary]] · [[Sources MOC]]
