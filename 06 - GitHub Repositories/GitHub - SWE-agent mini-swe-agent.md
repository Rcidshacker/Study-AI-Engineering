---
title: GitHub - SWE-agent mini-swe-agent
aliases:
  - mini-swe-agent
tags:
  - github
  - repository
  - loop-engineering
  - harness-engineering
  - evergreen
repo: SWE-agent/mini-swe-agent
url: https://github.com/SWE-agent/mini-swe-agent
stars: 6938
license: MIT
language: Python
verified: 2026-09-04
status: evergreen
created: 2026-09-04
updated: 2026-09-04
---

# GitHub — SWE-agent/mini-swe-agent

## Overview

The minimal AI software-engineering agent: roughly **100 lines of Python** for the agent
class, plus a little more for the environment, model and run script.

## GitHub

`https://github.com/SWE-agent/mini-swe-agent` — verified via the GitHub API **2026-09-04**:

| Field | Value |
|---|---|
| Stars / forks | 6,938 / 965 |
| Language / licence | Python / MIT |
| Created | 2025-06-28 |
| Last push | 2026-09-03 |
| Open issues | 59 |
| Note | README warns this is **v2**; v1 lives on the `v1` branch |

## Why it matters

`[FACT — README claims]` It scores **>74% on SWE-bench Verified**, and the README lists
adoption by Meta, NVIDIA, IBM, Nebius, Anyscale, Princeton and Stanford. Its authors built
SWE-bench and SWE-agent in 2024 and then asked: *"What if our agent was 100x simpler, and
still worked nearly as well?"*

`[INFERENCE]` For this vault its value is not the score — it is that **it is short enough to
read completely in one sitting**, so you can see exactly what an agent loop must contain and,
more usefully, what it does *not* need. Every extra thing your harness has, you now have to
justify against this baseline.

## Architecture

Root layout: `src/`, `tests/`, `docs/`, plus **both** an `AGENTS.md` and a `CLAUDE.md` —
the repo is itself harnessed.

The whole design is four replaceable pieces:

```text
Model        ── litellm wrapper, provider-agnostic
Environment  ── executes an action, returns output (local / docker / …)
Agent        ── the loop and its limits          ← src/minisweagent/agents/default.py
Run script   ── wires the three together
```

## Important files

| File | Why |
|---|---|
| `src/minisweagent/agents/default.py` | **the whole loop**, ~200 lines including config and error handling |
| `src/minisweagent/environments/local.py` | the minimum an environment must offer |
| `src/minisweagent/models/litellm_model.py` | provider abstraction |
| `src/minisweagent/run/hello_world.py` | the smallest working wiring |
| `AGENTS.md` / `CLAUDE.md` | its own outer harness |

## The loop, as actually written `[FACT — read from source 2026-09-04]`

```python
def run(self, task="", **kwargs) -> dict:
    self.messages = []
    self.add_messages(system_message, instance_message)   # two messages, then loop
    while True:
        try:
            self.step()
            self.n_consecutive_format_errors = 0          # reset on any clean step
        except FormatError as e:      ...                  # count, maybe exit
        except InterruptAgentFlow as e: self.add_messages(*e.messages)
        except Exception as e:        self.handle_uncaught_exception(e); raise
        finally:
            self.save(self.config.output_path)             # trajectory saved EVERY step
        if self.messages[-1].get("role") == "exit":
            break
    return self.messages[-1].get("extra", {})

def step(self):
    return self.execute_actions(self.query())
```

Three things to notice:

1. **`step() = execute_actions(query())`.** That is the entire ReAct loop. Everything else in
   the file is limits, error handling and templating. See [[Agent Loops]].
2. **Termination is a message role.** The loop ends when the last message has
   `role == "exit"`. Every stopping path — success, budget, timeout, repeated malformed
   output — converges on producing that one message. A single, uniform exit channel.
3. **The trajectory is saved in a `finally` block**, so it survives crashes. Observability is
   not optional or bolted on. See [[Agent Observability]].

## Harness engineering lessons

- **A minimal harness has a Model, an Environment, and templates — nothing more.** No graph,
  no memory store, no orchestration layer. Anything beyond this is an addition you must
  justify. See [[When Not to Build a Harness]].
- **The environment is the harness boundary.** `env.execute(action)` is the only way the agent
  affects the world, so swapping `local` for `docker` changes the entire safety posture
  without touching the agent. See [[Sandboxing and Permissions]].
- **Prompts are Jinja templates rendered with `StrictUndefined`** — an unknown variable is an
  error, not a silent blank. `[INFERENCE]` A small but excellent instance of
  [[Executable Rules Beat Written Rules]]: the harness fails loudly on its own misconfiguration.

## Loop engineering lessons

`[FACT]` `AgentConfig` declares **four independent stopping conditions**, and this is the
single most copyable thing in the repo:

| Field | Default | Guards against |
|---|---|---|
| `step_limit` | 0 (off) | endless iteration |
| `cost_limit` | **3.0** | runaway spend |
| `wall_time_limit_seconds` | 0 (off) | hanging |
| `max_consecutive_format_errors` | **3** | a model stuck emitting unparseable output |

Two details worth stealing:

- **Cost is checked *before* the call, not after.** Limits are enforced at the boundary.
- **The format-error counter is `n_consecutive`, and resets on any clean step.** It detects
  *being stuck*, not *having ever failed*. A cumulative counter would kill healthy long runs.
  `[INFERENCE]` This is the cheapest good stuck-detector I have seen.

Exit statuses are distinct and recorded — `LimitsExceeded`, `TimeExceeded`,
`RepeatedFormatError`, or the exception class name. **"The loop stopped" is never conflated
with "the task succeeded."** See [[Stopping Conditions]] and [[Retry Strategies]].

## Graph engineering lessons

None — deliberately. It is a single loop with a single agent, and it performs. `[INFERENCE]`
Keep this repo in mind as the counter-example whenever a multi-agent graph is proposed: a
100-line single loop reaches >74% on a serious benchmark. See [[Graph vs Workflow]].

## Claude Code relevance

Claude Code's built-in loop is a far more elaborate relative of this one. Reading
`default.py` gives you an accurate mental model of the **inner harness** you are building on
top of — what an agent loop fundamentally is, before product features are added. See
[[Inner Harness vs Outer Harness]] and [[Claude Code Architecture]].

## What to study

1. `default.py` end to end — it is short; read all of it.
2. The `AgentConfig` limits, then port them to your own loops.
3. `environments/local.py` versus a container environment — the safety delta.
4. Its own `AGENTS.md`/`CLAUDE.md` as a compact instruction-file example.

## Limitations

- Deliberately minimal: **no persistent memory, no multi-session continuity, no subagents.**
  For long-running work you must add what
  [[Source - Anthropic Effective Harnesses for Long-Running Agents]] describes.
- Benchmark performance is reported by its authors; treat as vendor-reported.
- v1 → v2 was a breaking change; check which version any tutorial refers to.

## Related

- [[Agent Loops]] · [[Stopping Conditions]] · [[Loop Types]] · [[When Not to Build a Harness]]
- [[Agent Evaluation]] · [[Repository Index]]
