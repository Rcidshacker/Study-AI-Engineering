---
title: Ralph Loop
aliases:
  - Ralph Wiggum Loop
  - Ralph
tags:
  - loop-engineering
  - pattern
  - evergreen
status: evergreen
confidence: high
created: 2026-09-04
updated: 2026-09-04
---

# Ralph Loop

> [!abstract] One line
> Run a **fresh agent instance** against the same prompt, over and over, until a completion
> signal appears — and keep all memory *outside* the agent, in git and two files.

The pattern is named after Geoffrey Huntley's "Ralph" write-up. `[FACT]` The most widely-used
implementation is `snarktank/ralph`, and `[FACT]`
[[Source - OpenAI Harness Engineering]] explicitly names it — their PR-completion loop
"effectively is a Ralph Wiggum Loop."

Source inspected directly on 2026-09-04: see [[GitHub - snarktank ralph]].

---

## The counter-intuitive core

Almost every instinct says a long task needs a long-lived agent with accumulating context.
Ralph does the opposite:

> **Each iteration is a fresh instance with clean context. Memory persists via git history,
> `progress.txt`, and `prd.json`.** `[FACT — repo README]`

`[INFERENCE]` Why this beats one long session:

- **Context rot never accumulates.** Iteration 40 starts as clean as iteration 1.
- **No compaction lossiness.** Compaction summarises and drops detail unpredictably;
  a file does not.
- **State becomes inspectable and editable.** You can read `progress.txt`, fix a wrong entry
  in `prd.json`, and the next iteration picks up your correction.
- **Failure is bounded.** A bad iteration costs one iteration. Git makes it revertible.

This is [[Context Window as a Budget]] taken to its conclusion: stop trying to keep things in
context; put them on disk and re-read what matters.

---

## The actual control loop `[FACT — read from `ralph.sh`]`

Stripped to essentials:

```bash
for i in $(seq 1 $MAX_ITERATIONS); do
  OUTPUT=$(claude --dangerously-skip-permissions --print < "$SCRIPT_DIR/CLAUDE.md" 2>&1 | tee /dev/stderr) || true

  if echo "$OUTPUT" | grep -q "<promise>COMPLETE</promise>"; then
    echo "Ralph completed all tasks!"; exit 0
  fi
done
exit 1   # hit max iterations without completing
```

Three design details worth stealing:

1. **`MAX_ITERATIONS` defaults to 10.** A hard budget, always. See [[Stopping Conditions]].
2. **`|| true`** — a failed iteration does not kill the loop. Failure is expected and survivable.
3. **Exit 1 on exhaustion.** Running out of budget is a *distinct outcome* from success, and
   it is reported as one. Do not let "the loop stopped" mean "the work is done."

It also **archives** the previous run's `prd.json` and `progress.txt` when the branch changes,
then resets the progress log — a clean-state ritual at the loop boundary.

---

## The stopping condition `[FACT — from the repo's `CLAUDE.md`]`

> "After completing a user story, check if ALL stories have `passes: true`. If ALL stories are
> complete and passing, reply with `<promise>COMPLETE</promise>`."

So termination is: **a sentinel string, emitted only when a checkable data structure says
every item is done.** Not "the agent feels finished."

`[OPINION]` This is weaker than it looks — the *agent* still sets `passes: true`. The judge and
the judged are not fully separated, which is exactly the gap [[Generator Evaluator Separation]]
warns about. Ralph mitigates it with the quality-gate requirement ("ALL commits must pass
typecheck, lint, test"; "Do NOT commit broken code") rather than eliminating it. If you adopt
Ralph for anything consequential, **add an independent checker** that can flip `passes` back
to `false`.

---

## The agent contract `[FACT]`

Each iteration, the agent must:

1. Read `prd.json`.
2. Read `progress.txt` — **Codebase Patterns section first**.
3. Verify it is on the branch named in the PRD; check out or create from main.
4. Pick the **highest priority** story where `passes: false`.
5. Implement **that single story** — "Work on ONE story per iteration."
6. Run quality checks (typecheck, lint, test).
7. Update nearby `CLAUDE.md` files if a reusable pattern was discovered.
8. Commit everything: `feat: [Story ID] - [Story Title]`.
9. Set `passes: true` for that story.
10. **Append** to `progress.txt` — "never replace, always append."

---

## The learning mechanism — the most-copied part

Ralph has two distinct memory tiers, and the distinction is the clever bit:

| Tier | Location | Content | Lifetime |
|---|---|---|---|
| **Episodic** | `progress.txt` body, appended | what was implemented, files changed, gotchas hit | this run |
| **Semantic** | `## Codebase Patterns` at the **top** of `progress.txt` | general, reusable facts about the codebase | promoted, read first |
| **Durable** | nearby `CLAUDE.md` files | knowledge that outlives the run | permanent |

The repo is explicit that only **"general and reusable"** items get promoted, and that
`CLAUDE.md` should not receive "story-specific implementation details" or "temporary debugging
notes."

`[INFERENCE]` This three-tier promotion — *observation → consolidated pattern → permanent
instruction* — is a genuine [[Fix the Class Not the Instance]] loop running **inside** the
agent loop, and it is the reason a Ralph run gets better as it goes despite each iteration
starting blind. It is the single most transferable idea in the repo.

---

## Safety `[FACT]`

The loop runs `claude --dangerously-skip-permissions`, and the Amp path passes
`--dangerously-allow-all`.

> [!warning] Do not run this on a repo you care about without isolation
> Permission checks are the outer harness's main containment mechanism, and this pattern
> switches them off so the loop can run unattended. Mitigations before you use it:
> - run inside a **container or VM**, or at minimum a dedicated **git worktree**
> - operate on a **dedicated branch** (Ralph reads `branchName` from the PRD — use it)
> - ensure the working tree is committed before starting
> - keep `MAX_ITERATIONS` small on the first runs and read `progress.txt` after each
>
> See [[Sandboxing and Permissions]] and [[Worktree Isolation]].

---

## When to use it

**Good fit:** a well-specified backlog of independent, individually-testable items; a project
where the test suite is the arbiter; overnight or unattended work.

**Bad fit:** exploratory work with no definition of done; a single large interdependent
change; anything where a wrong step is expensive to undo.

See [[Loop Types]] — Ralph is a goal loop implemented as repeated independent runs, which
works *only* because of the external state.

---

## Related

- [[Loop Engineering]] · [[Loop Types]] · [[Stopping Conditions]] · [[External State]]
- [[Feature List as Harness Primitive]] · [[Generator Evaluator Separation]] · [[Clean State Ritual]]
- [[GitHub - snarktank ralph]] · [[Source - OpenAI Harness Engineering]]
