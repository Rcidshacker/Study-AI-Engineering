---
title: The Repository as System of Record
aliases:
  - The repo is the spec
  - The Filesystem as Harness Primitive
  - What the Model Will Absorb
tags:
  - harness-engineering
  - agent-state
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# The Repository as System of Record

> [!abstract] One line
> Anything the agent cannot access in-context does not exist. That single constraint reorganises
> where a team's knowledge has to live.

---

## The claim `[FACT]`

[[Source - OpenAI Harness Engineering]]:

> "From the agent's point of view, **anything it can't access in-context while running
> effectively doesn't exist.** Knowledge that lives in Google Docs, chat threads, or people's
> heads are not accessible to the system. Repository-local, versioned artifacts (e.g. code,
> markdown, schemas, executable plans) are all it can see."

And the consequence for how teams work:

> "That Slack discussion that aligned the team on an architectural pattern? If it isn't
> discoverable to the agent, it's illegible in the same way it would be **unknown to a new hire
> joining three months later**."

`[INFERENCE]` The new-hire framing is the useful one. Agent legibility is not an exotic
requirement; it is onboarding quality, enforced continuously and without the courtesy of
someone asking a question.

---

## Why the filesystem specifically `[FACT]`

[[Source - Anatomy of an Agent Harness]] argues it is "arguably the most foundational harness
primitive," for four reasons:

1. Models were trained on **billions of tokens of filesystem usage**, so the affordance is
   already native — no new interface has to be taught.
2. Work can be **incrementally offloaded** instead of held in context.
3. It is a **natural collaboration surface** — several agents and humans coordinate through
   shared files.
4. **Git adds versioning**: agents "track work, rollback errors, and branch experiments."

`[INFERENCE]` Reason 1 generalises into a design rule worth stating on its own: **prefer
interfaces the model has already seen millions of times** — files, bash, git, JSON, markdown —
over bespoke APIs it must be taught in-context. Every custom interface is a tax paid on every
session.

---

## Legibility as a design objective `[FACT]`

OpenAI optimised the repository first for *the agent's* legibility. Consequences they report:

- Favour **"boring" technologies** — "composability, api stability, and representation in the
  training set."
- Sometimes **reimplement a subset** of a library rather than work around opaque upstream
  behaviour. Named example: instead of a `p-limit`-style package, their own map-with-concurrency
  helper, 100% covered and natively instrumented.
- Output "does not always match human stylistic preferences, and that's okay."

`[INFERENCE]` The middle bullet is the one that will feel wrong to experienced engineers, and
it is defensible only under their specific conditions: a fully agent-generated codebase where
maintenance cost is low and opacity cost is high. **Do not generalise it.** The transferable
version is the first bullet: boring, stable, well-represented technology is easier for an agent
to reason about, and that is now a selection criterion alongside the usual ones.

---

## What has to move into the repo

`[INFERENCE]` The audit is simple — for each thing the agent needs to know, ask *where does it
live?*

| Knowledge | Usually lives | Must live |
|---|---|---|
| Why the architecture is this way | a design review, someone's memory | `docs/design-docs/` |
| What we are building next | a ticket tracker | `docs/exec-plans/active/` |
| What "done" means | in your head | `feature_list.json` |
| What is broken and known | a Slack thread | `docs/exec-plans/tech-debt-tracker.md` |
| How to run this thing | tribal knowledge | `init.sh`, `CLAUDE.md` |
| The database shape | the database | `docs/generated/db-schema.md` |
| Conventions | code review comments | rules, and **checks** |

`[FACT]` OpenAI's published layout puts all of this in a structured `docs/` tree, with a short
instruction file as the map. See [[Instruction File Design]].

---

## The feedback direction `[FACT]`

> "Human taste is fed back into the system continuously. Review comments, refactoring pull
> requests, and user-facing bugs are captured as documentation updates or encoded directly into
> tooling. When documentation falls short, we promote the rule into code."

`[INFERENCE]` So the repository is not a static archive; it is where corrections **accumulate**.
That is the mechanism behind [[Fix the Class Not the Instance]] — without a system of record,
every correction is spent once and lost.

---

## The test

> Delete every chat log, close every document, and send the team home. Could an agent — or a new
> hire — build the next feature correctly from the repository alone?

`[INFERENCE]` Whatever you had to go and ask someone is the gap. That is the work.

---

## Related

- [[External State]] · [[Agent State]] · [[Instruction File Design]] · [[Context Window as a Budget]]
- [[Fix the Class Not the Instance]] · [[Harness Components]] · [[Production Coding Agent]]
- [[Source - OpenAI Harness Engineering]] · [[Source - Anatomy of an Agent Harness]]
