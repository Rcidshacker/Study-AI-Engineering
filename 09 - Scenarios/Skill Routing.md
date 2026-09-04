---
title: Skill Routing
aliases:
  - Skill Router
  - Scenario 6
  - Routing 20+ skills
  - Skill Graphs
tags:
  - scenario
  - claude-code
  - graph-engineering
  - context-engineering
  - evergreen
status: evergreen
confidence: high
verified: 2026-09-04
created: 2026-09-04
updated: 2026-09-04
---

# Scenario — Skill Routing

> [!abstract] The task
> You have 20+ skills. Something must get from **user request → intent → the right skill →
> supporting skills → tools → execution**. Is that a graph, a routing layer, or something
> simpler?

> [!info] Verification
> The mechanics below were read from the official skills documentation at
> `code.claude.com/docs/en/skills.md` on **2026-09-04**. This scenario has an unusually
> concrete answer because the routing layer is documented, including its budget.

---

## The answer, up front

**Build nothing.** The router already exists, it is the model, and it is better at intent
matching than anything you would write. At 20+ skills your problem is not routing — it is
**description budget**, and it has documented settings.

`[INFERENCE]` This is the single most useful "don't build it" finding in the vault. A
hand-written intent classifier in front of a model that already does intent classification adds
a layer that can be wrong, costs a call, and must be maintained as skills change.

---

## How routing actually works `[FACT]`

> "In a regular session, **skill descriptions are loaded into context** so Claude knows what's
> available, but **full skill content only loads when invoked**."

So the mechanism is:

```text
every session:   [ name + description ] × N skills   ← always in context
on invocation:   the full SKILL.md body               ← loaded once, then persists
```

Two consequences that decide everything else:

1. **The `description` *is* the routing table.** Matching happens against it. `[FACT]` The
   troubleshooting guidance is literally "check the description includes keywords users would
   naturally say."
2. **A loaded skill stays loaded.** `[FACT]` "The rendered `SKILL.md` content enters the
   conversation as a single message and **stays there across later turns**… every line is a
   recurring token cost." Claude Code does not re-read the file, so write standing instructions,
   not one-time steps.

---

## The real failure at 20+ skills `[FACT]`

This is the documented mechanic almost nobody knows about:

> "The listing always contains every skill name, but **if you have many skills, Claude Code
> shortens descriptions to fit the listing's character budget, which can strip the keywords
> Claude needs to match your request.** The budget scales at **1% of the model's context
> window**. When the listing overflows, Claude Code **drops descriptions starting with the
> skills you invoke least**, so the skills you use most keep their full text."

Read that carefully. At scale:

- Names always survive; **descriptions are what get truncated**.
- The eviction order is **least-invoked first** — so a rarely-used skill silently loses exactly
  the keywords that would have caused it to be used. `[INFERENCE]` A quiet feedback loop:
  under-used skills become less discoverable, which makes them less used.
- Per-entry text is capped at **1,536 characters** regardless of budget
  (`skillListingMaxDescChars`).

**This — not intent classification — is what breaks at 20+ skills.**

---

## Diagnose before you touch anything `[FACT]`

| Tool | Tells you |
|---|---|
| `/doctor` | an estimate of the listing's context cost, and its **biggest contributors** |
| `/context` | the **Skills** row: the listing size *after* the budget is applied — what the model actually receives |
| `--debug` | a warning written to the debug log when the listing overflows |
| "What skills are available?" | whether a skill is visible at all |

`[INFERENCE]` Run `/doctor` first. If your listing is inside budget, you do not have a routing
problem, and any router you build will be solving a problem you do not have.

---

## The five levers, cheapest first `[FACT]`

### 1. Write the description for the router

Put the **key use case first** — each entry is truncated from the end, so a description that
opens with preamble loses its keywords first. Include the words a user would actually say.

### 2. Take skills out of the listing that never needed to be in it

```yaml
---
name: deploy
description: Deploy the application to production
disable-model-invocation: true
---
```

`[FACT]` With `disable-model-invocation: true`, the **description is not in context at all** —
`/deploy` still works, and the listing budget is freed. Documented use: "workflows with side
effects or that you want to control timing, like `/commit`, `/deploy`, or
`/send-slack-message`. You don't want Claude deciding to deploy because your code looks ready."

`[INFERENCE]` This is the highest-value lever and it does double duty: it frees routing budget
**and** it is a safety control, since it removes a class of irreversible action from the
model's reach. See [[Sandboxing and Permissions]].

The inverse also exists: `user-invocable: false` for background knowledge that is not a
meaningful command — Claude can load it, you cannot type it.

### 3. Demote low-priority skills from settings `[FACT]`

`skillOverrides` in `.claude/settings.local.json`, four states:

| Value | Listed to Claude | In `/` menu |
|---|---|---|
| `"on"` | name **and** description | yes |
| `"name-only"` | name only | yes |
| `"user-invocable-only"` | hidden | yes |
| `"off"` | hidden | hidden |

```json
{ "skillOverrides": { "legacy-context": "name-only", "deploy": "off" } }
```

`[FACT]` The `/skills` menu writes this for you — highlight, press `Space` to cycle, `Esc` to
save. `[INFERENCE]` Use it for skills in a shared repo whose `SKILL.md` you should not edit.

### 4. Raise the budget `[FACT]`

`skillListingBudgetFraction` (e.g. `0.02` = 2%) or `SLASH_COMMAND_TOOL_CHAR_BUDGET` for a fixed
character count. `disableBundledSkills` removes the shipped ones.

`[INFERENCE]` Do this **last**. The budget exists because listing text competes with your task,
your code and your docs — see [[Context Window as a Budget]]. Raising it trades routing accuracy
against everything else in the window, which is a real trade and not a free win.

### 5. Split the body out of the entrypoint `[FACT]`

Keep `SKILL.md` under 500 lines; move reference material to separate files the skill points at,
loaded only if needed. And `context: fork` runs a skill in a subagent, keeping its content out
of the main conversation entirely.

---

## The two symptoms, and their fixes `[FACT]`

| Symptom | Fix |
|---|---|
| **Skill doesn't trigger** | description lacks the words users say; check it appears in "What skills are available?"; check the frontmatter parses — malformed YAML loads the body with **empty metadata**, so `/name` works but Claude has nothing to match on. `claude plugin validate .claude/skills` finds these |
| **Skill triggers too often** | make the description more specific; or `disable-model-invocation: true` |

`[INFERENCE]` The malformed-frontmatter case is nasty precisely because it half-works: manual
invocation succeeds, so the skill looks installed while being invisible to routing. Validate.

---

## When a real router *is* justified

`[INFERENCE]` Three cases, all narrow:

| Case | Why the model is not enough | Build |
|---|---|---|
| **Routing must be deterministic** — compliance, audit, "this request type must always take this path" | the model's choice is not reproducible | a script with explicit dispatch; skills become the leaves |
| **Cost routing** — cheap path for common requests | the model does not know your budget | a pre-dispatch layer choosing model or path |
| **Hundreds of skills, in domains** | the listing cannot hold them all even after tuning | one router **subagent** per domain, each with a scoped skill set |

The third is the only one that is genuinely a graph, and note what it actually is:
[[Context Window as a Budget|a context-budget fix]] wearing a graph costume. See
[[Claude Code Graphs]].

---

## The proposed pipeline, corrected

```text
       PROPOSED                              ACTUAL
   User Request                          User Request
        ↓                                     ↓
     Intent          ─────────►      [ model matches against the
        ↓                              description listing —
   Required Skill                       already in context ]
        ↓                                     ↓
  Supporting Skills                    skill body loads, persists
        ↓                                     ↓
      Tools                            its instructions shape tool use
        ↓                                     ↓
    Execution                              Execution
```

`[INFERENCE]` Every arrow in the left column already exists. What you own is **the text the
matching happens against, and how much of it survives the budget.** That is context
engineering, not orchestration — which is why this scenario resolves to "write better
descriptions and prune the listing" rather than to a diagram.

---

## The checklist

- [ ] Run `/doctor`; note the listing cost and top contributors
- [ ] Check the `Skills` row in `/context`
- [ ] Any skill with side effects → `disable-model-invocation: true`
- [ ] Any background-knowledge skill → `user-invocable: false`
- [ ] Rewrite descriptions: key use case first, real user words
- [ ] Demote the rest with `skillOverrides: "name-only"`
- [ ] `claude plugin validate .claude/skills`
- [ ] Only now consider raising `skillListingBudgetFraction`

---

## Related

- [[Context Window as a Budget]] · [[Claude Code Implementation Notes]] · [[Claude Code Graphs]]
- [[Sandboxing and Permissions]] · [[When Not to Build a Harness]] · [[Agent Orchestration]]
- [[Scenarios MOC]]
