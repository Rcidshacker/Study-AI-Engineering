---
title: Harness Engineering
aliases:
  - Agent Harness
  - Agent Scaffolding
  - Harness Design
tags:
  - harness-engineering
  - agent-engineering
  - ai-engineering
  - evergreen
status: evergreen
confidence: high
created: 2026-09-03
updated: 2026-09-03
---

# Harness Engineering

> [!abstract] One line
> Harness engineering is the practice of building **everything around the model** so that a non-deterministic text predictor behaves like a reliable engineer on your codebase.

**Evidence legend used across this vault:** `[FACT]` documented in a primary source · `[PRACTICE]` widely-reported community practice · `[OPINION]` a named expert's position · `[INFERENCE]` my own synthesis · `[UNVERIFIED]` could not confirm.

---

## 1. The definition

`[FACT]` The canonical formulation is an equation:

> **Agent = Model + Harness**

Vivek Trivedy of LangChain defines the harness as "every piece of code, configuration, and execution logic that isn't the model itself," and frames the split as: the model contains the intelligence, the harness makes that intelligence useful ([[Source - Anatomy of an Agent Harness]], 10 Mar 2026).

`[FACT]` Birgitta Böckeler defines it more tightly for our purposes — the harness is "everything in an AI agent except the model itself," and for *coding agent users* specifically it narrows to the controls you build around the agent ([[Source - Harness Engineering for Coding Agent Users]], 2 Apr 2026).

`[FACT]` The English Wikipedia article [[Source - Wikipedia Agent Harness]] defines an agent harness (a.k.a. *agent scaffolding*) as the software infrastructure surrounding an LLM that lets it operate as an agent — managing tool use, memory, state persistence, execution environments and feedback loops, "as opposed to the model's internal reasoning."

### Why a harness is needed at all

`[FACT]` Two properties of the underlying model create the entire discipline:

1. **The model is stateless.** It has no memory between calls. Every notion of "where we are in the task" has to be reconstructed or stored by something outside the model.
2. **The model only emits text.** It cannot open a file, run a test, or make a commit. Every *action* is something the harness offers, executes, and reports back on.

`[FACT]` Wikipedia makes the consequence explicit: rather than repeatedly re-reading an ever-growing transcript inside the context window, a harness can offload record-keeping into a structured software environment that manages the agent's state. This is the deepest idea in the field — see [[Context Window as a Budget]].

`[FACT]` And the honest scope limit: "A minimal harness is unnecessary for a single prompt-and-response exchange, but becomes important as tasks grow multi-step, tool-oriented, or long-running." Harness engineering is **not** justified for one-shot prompting. See [[When Not to Build a Harness]].

---

## 2. Is the term established?

`[FACT]` **It is genuinely emerging, and it is roughly one year old.** Per Wikipedia, "harness engineering" emerged in **early 2026**, and attribution is *contested* between several near-simultaneous accounts:

| Claimant | Contribution | Date |
|---|---|---|
| Mitchell Hashimoto | Blog post on engineering permanent fixes into an agent's environment after mistakes | Feb 2026 |
| Vivek Trivedy (LangChain) | "The Anatomy of an Agent Harness" — derives components from `Agent = Model + Harness` | 10 Mar 2026 |
| OpenAI | Engineering report on large codebases built by coding agents | 2026 |

`[FACT]` The *underlying idea* is older than the term. The UK's AI Security Institute described an AI agent as the model **plus the scaffolding** back in **2023** — so "scaffolding" is the older word for the same thing, and "harness" is the 2026 rebrand.

`[FACT]` The word "harness" is itself borrowed from established software practice: a **test harness**, and later an **evaluation harness** for benchmarking models. See [[Lineage of the Word Harness]].

> [!warning] Terminology honesty
> This is a **young, contested term**, not a settled discipline with textbooks. Anyone presenting a single canonical definition of harness engineering is overstating consensus. What *is* settled is the underlying decomposition (`model` vs `everything else`), which predates the term by years.

---

## 3. Böckeler's model: guides and sensors

This is the most useful mental model I found, and the one I'd internalise first.

`[FACT]` Böckeler frames the harness cybernetically — as **a governor**, combining feed-forward and feedback to regulate the codebase toward its desired state. It splits into two control types:

```
                         ┌──────────────────────┐
        GUIDES ─────────►│                      │
   (feedforward:         │     CODING AGENT     │
    steer before acting) │                      │
                         └──────────┬───────────┘
                                    │ acts on codebase
                                    ▼
                         ┌──────────────────────┐
        SENSORS ◄────────│   RESULTING CHANGE   │
   (feedback:            └──────────────────────┘
    observe after acting,
    enable self-correction)
```

- **Guides** — "anticipate the agent's behaviour and aim to steer it before it acts." Examples she names: `AGENTS.md`, skills, bootstrap scripts, architecture docs, LSP integration, OpenRewrite recipes, MCP access to knowledge systems.
- **Sensors** — "observe after the agent acts and help it self-correct." Examples she names: pre-commit hooks running structural tests (ArchUnit), linters (eslint), static analysis (semgrep), coverage checks, dependency analysis (dep-cruiser), mutation testing, custom AI code-review skills.

`[FACT]` Both guides and sensors come in two flavours:

| | Computational | Inferential |
|---|---|---|
| **What** | Tests, linters, type checkers | Semantic analysis, AI code review, LLM-as-judge |
| **Runs on** | CPU, deterministic | GPU/NPU, non-deterministic |
| **Speed** | Fast | Slow |
| **Trust** | High | Needs its own verification |

`[INFERENCE]` The practical rule falling out of this: **prefer computational controls wherever a computational control is possible.** A type checker is a better sensor than an LLM reviewer for the same class of error, because it is fast, free, and cannot be talked out of its opinion. Reach for inferential controls only for things no compiler can check.

### Böckeler's three regulation domains

`[FACT]` She organises harness work by *what* it regulates:

1. **Maintainability harness** — internal code quality.
2. **Architecture fitness harness** — architectural characteristics, checked via *fitness functions*.
3. **Behaviour harness** — functional application behaviour.

`[FACT]` And she is candid that domain 3 is the unsolved one: "We still have a lot to do to figure out good harnesses for functional behaviour." She warns the common approach "puts a lot of faith into the AI-generated tests, that's not good enough yet." See [[The Verification Gap]].

### Inner harness vs outer harness

`[FACT]` Böckeler distinguishes (per Wikipedia's summary of her work):

- **Inner harness** — supplied by the model builder: the agent SDK, the built-in tool loop, compaction. You mostly don't control this.
- **Outer harness** — assembled by you: instruction files, MCP servers, hooks, CI gates.

`[INFERENCE]` For a Claude Code user this is the single most clarifying distinction in the whole topic. Claude Code *is* the inner harness. `CLAUDE.md`, `.claude/`, your hooks and your CI are the outer harness. **Harness engineering, for you, means outer-harness engineering.** See [[Inner Harness vs Outer Harness]].

---

## 4. Trivedy's component inventory

`[FACT]` The LangChain piece enumerates harness components more exhaustively than anyone else I found. Consolidated:

| Component | What it does |
|---|---|
| System prompts | Guide model behaviour |
| Tools / Skills / MCP | Grant capabilities; Skills use progressive disclosure to reduce context rot |
| Filesystem abstractions | Durable storage, workspace, state across sessions, multi-agent coordination |
| Git versioning | Track work, roll back, branch experiments |
| Bash / code execution | The general-purpose escape hatch — solve problems by writing code |
| Sandboxes | Safe isolated execution, security controls, scaling |
| Default tooling | Pre-installed runtimes, CLIs, browsers, test runners for verification |
| Memory standards | `AGENTS.md`-style files for continual learning via context injection |
| Web search / doc MCP | Reach past the knowledge cutoff |
| Compaction logic | Summarise context when the window fills |
| Tool-call offloading | Store large outputs on the filesystem, keep context clean |
| Planning tools | Decompose objectives into steps |
| Self-verification loops | Test runners, logs, screenshots → detect and correct errors |
| Ralph loops | Hooks intercept model exit, reinject the prompt into a clean context |
| Subagent spawning | Parallel work in isolated contexts |
| Orchestration logic | Model routing and handoffs |
| Hooks / middleware | Deterministic execution: compaction, continuation, lint checks |

See [[Harness Component Inventory]] for each of these mapped onto a concrete Claude Code mechanism.

---

## 5. The evidence that the harness matters more than you'd think

`[FACT]` Trivedy reports that LangChain moved their coding agent from roughly **Top 30 to Top 5** on the Terminal-Bench 2.0 leaderboard by optimising **only the harness** — the model was unchanged. He also notes the same model scores differently across different harnesses.

`[FACT]` He draws the counterintuitive conclusion: "The best harness for your task isn't necessarily the one a model was trained with."

`[INFERENCE]` This is the empirical justification for the whole discipline. If harness changes alone move a benchmark rank by ~25 places at fixed model, then for a practitioner who cannot change the model weights, **the harness is the only lever with that much leverage.** See [[Harness Beats Model Choice]].

---

## 6. Harness engineering vs the neighbouring terms

This is the question most people get wrong. The honest answer is that these are **nested scopes, not rivals**, and the boundaries are genuinely fuzzy.

```
┌─────────────────────────────────────────────────────┐
│ AGENT ENGINEERING  (the whole discipline)           │
│                                                     │
│  ┌───────────────────────────────────────────────┐  │
│  │ HARNESS ENGINEERING                           │  │
│  │  everything around the model                  │  │
│  │                                               │  │
│  │   ┌─────────────────────────────────────┐     │  │
│  │   │ CONTEXT ENGINEERING                 │     │  │
│  │   │  what the model can see, when       │     │  │
│  │   │                                     │     │  │
│  │   │   ┌───────────────────────────┐     │     │  │
│  │   │   │ PROMPT ENGINEERING        │     │     │  │
│  │   │   │  wording of one request   │     │     │  │
│  │   │   └───────────────────────────┘     │     │  │
│  │   └─────────────────────────────────────┘     │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

| Discipline | Scope | Unit of work |
|---|---|---|
| **Prompt engineering** | Wording of a single request | A string |
| **Context engineering** | What information is in the window, and when | The window |
| **Harness engineering** | The whole environment: tools, permissions, sensors, state, recovery | The system around one agent |
| **[[Loop Engineering]]** | How runs repeat, chain, and terminate over time | The schedule / the repetition |
| **Agent engineering** | All of it, including model and eval selection | The product |

`[FACT]` Böckeler explicitly places the two in relation: "Context engineering provides us with the means to make guides and sensors available to the agent. Engineering a user harness for a coding agent is a specific form of context engineering."

> [!note] A real disagreement, recorded honestly
> Böckeler calls harness engineering **a specific form of context engineering** (harness ⊂ context). Trivedy's component list treats context management (compaction, offloading) as **one component among many inside** the harness (context ⊂ harness). These are opposite containment claims.
>
> `[INFERENCE]` They are not really contradicting each other — Böckeler is describing *the mechanism by which* guides and sensors reach the model (which is indeed context), while Trivedy is describing *the set of things you build* (of which context management is one). But you should know that **the nesting is not agreed on**, and treat any tidy hierarchy diagram — including mine above — as a teaching aid rather than a fact. See [[Harness vs Loop vs Graph]].

`[FACT]` Wikipedia offers a third framing: harness engineering is distinguished by accommodating **non-deterministic model behaviour** — recovering gracefully when models fabricate actions or *falsely report task completion*. `[INFERENCE]` I find this the sharpest discriminator of the three: prompt engineering assumes the model will comply; harness engineering assumes it will sometimes lie about being done, and builds for that. See [[False Completion]].

### vs. developer tooling

`[INFERENCE]` The distinction that actually matters in practice: **conventional developer tooling optimises for a human in the loop; harness tooling optimises for a machine reader that will act on the output unsupervised.** A linter whose output a human skims is dev tooling. The same linter wired to a hook that *blocks* the agent's commit is a harness sensor. The tool did not change; the consumer and the enforcement did. See [[Developer Tooling vs Harness Tooling]].

---

## 7. The steering loop: how you actually do harness engineering

`[FACT]` Böckeler's operating instruction is the practical core of the article:

> "The human's job in this is to steer the agent by iterating on the harness. Whenever an issue happens multiple times, the feedforward and feedback controls should be improved."

`[INFERENCE]` Restated as a rule I'd actually follow:

> **Fix the class, not the instance.** The first time the agent does something wrong, correct it in chat. The *second* time, stop correcting the output and change the harness — add the rule to `CLAUDE.md`, add the lint rule, add the hook. Chat corrections evaporate at the end of the session; harness changes are permanent.

This is the same idea as Mitchell Hashimoto's reported contribution (engineering permanent fixes into the agent's environment after mistakes) — see [[Fix the Class Not the Instance]].

`[FACT]` And the agent can build its own harness: Böckeler notes agents can "write structural tests, generate draft rules from observed patterns, scaffold custom linters, create how-to guides."

---

## 8. Limits, costs and open problems

Recorded because a knowledge base that only lists benefits is marketing.

`[FACT]` **Neither sensor type catches the expensive failures.** Böckeler: computational and inferential sensors alike miss "misdiagnosis of issues, overengineering and unnecessary features, misunderstood instructions." These are precisely the failures that cost the most to unwind. `[INFERENCE]` So the harness reduces the *rate* of cheap errors and barely touches the rate of expensive ones — which is an argument for keeping human review on *intent*, and delegating to the harness only the checking of *form*.

`[FACT]` **Not every codebase can be harnessed equally.** "Not every codebase is equally amenable to harnessing," and legacy systems face what she calls the harder problem: **the harness is most needed where it is hardest to build.**

`[FACT]` **Harness templates rot.** She expects shared harness templates to hit the same versioning and contribution problems as service templates, "maybe even worse with non-deterministic guides and sensors that are harder to test."

`[FACT]` **Building it is expensive.** "It is expensive, so we have to prioritise."

`[FACT]` **Model–harness overfitting.** Trivedy: "Training with a harness in the loop creates this overfitting" — a model trained alongside a particular tool (his example: `apply_patch`) can degrade when that tool's shape changes. `[INFERENCE]` Practical consequence: a clever custom replacement for a built-in tool can make things *worse*, because the model has been trained against the built-in's exact shape.

`[FACT]` **Some of the harness will be absorbed by the model.** Trivedy expects capabilities like planning and self-verification to migrate into base models over time — while arguing harness engineering stays useful anyway, by analogy with prompt engineering.

`[FACT]` **Open research problems** he names as unsolved: orchestrating hundreds of parallel agents on a shared codebase; self-trace analysis to identify failure modes; dynamic just-in-time tool and context assembly.

> [!question] The bitter-lesson objection
> The strongest argument *against* investing in harnesses: every capability you scaffold today is a capability the next model may ship natively, making your scaffolding dead weight. `[INFERENCE]` My read is that this argues against building *cognitive* scaffolding (planning frameworks, reasoning templates — the model gets better at these) but **not** against building *environmental* scaffolding (your tests, your permissions, your CI gates). No model release will ever ship knowledge of *your* repo's invariants. Invest on the environmental side of the line. See [[What the Model Will Absorb]].

---

## 9. Where this goes next

- The components, one note each → [[Harness Component Inventory]]
- Doing it in Claude Code specifically → [[Claude Code as a Harness]]
- The layer above → [[Loop Engineering]]
- The layer people over-reach for → [[Graph Engineering]]
- The unifying picture → [[The Unified Mental Model]]

## Sources

- [[Source - Harness Engineering for Coding Agent Users]] — Böckeler, martinfowler.com, 2 Apr 2026
- [[Source - Anatomy of an Agent Harness]] — Trivedy, LangChain, 10 Mar 2026
- [[Source - Wikipedia Agent Harness]] — Wikipedia, retrieved 3 Sep 2026
