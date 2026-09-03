---
title: Harness Engineering
aliases:
  - Agent Harness
  - Agent Scaffolding
tags:
  - ai-engineering
  - agent-engineering
  - harness-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Harness Engineering

## Definition

Harness engineering is the practice of designing and building the scaffolding (harness) around an AI model to enable it to operate effectively as an agent. The harness encompasses everything that is not the model itself: system prompts, tool configurations, context management, feedback loops, sandboxing, orchestration logic, and observability.

The core idea is encapsulated in the formula: **Agent = Model + Harness**.

## Origin and Popularization

The term "harness engineering" emerged in early 2026. Attribution is contested, but key contributors include:

- Viv Trivedy (LangChain) with the post "[The Anatomy of an Agent Harness](https://www.langchain.com/blog/the-anatomy-of-an-agent-harness)"
- Mitchell Hashimoto (HashiCorp) described engineering permanent fixes into an agent's environment after mistakes.
- Anthropic's engineering team published breakdowns of harness design for long-running work.
- Birgitta Böckeler provided a user-centric overview.

The concept builds on earlier notions of scaffolding in software testing (test harnesses), LLM benchmarking (evaluation harnesses), and reinforcement learning (agent environments).

## Problems Solved

Harness engineering addresses the limitations of raw language models when applied to real-world tasks:

- **Non-determinism**: Models produce variable outputs; harnesses introduce consistency through verification and retry loops.
- **Lack of state**: Models are stateless; harnesses provide memory and context persistence.
- **No tool use**: Models cannot execute actions; harnesses integrate tools, skills, and MCP servers.
- **Context limits**: Models have finite context windows; harnesses manage context via summarization, retrieval, and external storage.
- **Safety and reliability**: Harnesses enforce constraints, sandboxing, and validation to prevent harmful or incorrect outputs.
- **Orchestration**: For complex tasks, harnesses coordinate multiple agents, subagents, and workflow steps.

## Core Components of an Agent Harness

Based on authoritative sources (LangChain, Anthropic, OpenAI), a typical agent harness includes:

1. **System Prompts and Instructions** (e.g., `CLAUDE.md`, `AGENTS.md`)
   - Defines the agent's role, goals, and behavior guidelines.
   - Can include project-specific context and constraints.

2. **Tools, Skills, and MCP Servers**
   - Functions the agent can invoke (file operations, web search, code execution, etc.).
   - Skills are reusable, domain-specific tool sets.
   - MCP (Model Context Protocol) servers provide standardized tool interfaces.

3. **Bundled Infrastructure**
   - Filesystem access, sandboxing, browser capabilities, code execution environments.
   - Provides the agent with safe, controlled access to external resources.

4. **Orchestration Logic**
   - Manages subagent spawning, handoffs, and model routing.
   - Implements workflow patterns (sequential, parallel, conditional).

5. **Hooks and Middleware**
   - Deterministic execution aids: context compaction, continuation strategies, lint checks, formatting.
   - Runs before/after tool calls or agent turns to enforce constraints.

6. **Observability and Monitoring**
   - Logging, tracing, cost and latency metering, token usage tracking.
   - Enables debugging and performance optimization.

7. **Feedback Loops and Verification**
   - Mechanisms for self-correction: running tests, linting, human-in-the-loop review.
   - Includes automated verification (unit tests, type checkers) and manual verification.

8. **Memory and State Management**
   - Short-term (context window) and long-term (vector stores, databases) memory.
   - Persists information across agent invocations and sessions.

## Harness Engineering vs. Related Concepts

- **Prompt Engineering**: Focuses on crafting effective inputs to the model. Harness engineering is broader, encompassing the entire operational environment.
- **Context Engineering**: Deals with managing and optimizing the information provided to the model (retrieval, summarization, etc.). This is a subset of harness engineering.
- **Agent Engineering**: Often synonymous with harness engineering, but can also refer to the design of the agent's cognitive architecture (reasoning patterns, planning algorithms).
- **Loop Engineering**: Concerned with the iterative cycles (plan-act-observe) that the agent executes. Loops are a key component within the harness.

## Importance with Coding Agents

As coding agents (like Claude Code) become more capable, the limiting factor shifts from the model's raw intelligence to the effectiveness of the surrounding harness. A well-designed harness enables:

- Reduced supervision: Agents can work autonomously for longer periods.
- Increased reliability: Through verification and correction mechanisms.
- Better integration: With existing developer workflows (Git, CI/CD, testing frameworks).
- Specialization: Harnesses can be tailored for specific domains or tasks.

## Professional vs. AI Engineer vs. Vibe Coder Usage

- **Professional Software Engineers**: Focus on robustness, security, integration with existing systems, and maintaining harnesses as version-controlled infrastructure (e.g., treating `CLAUDE.md` and skills as code).
- **AI Engineers**: Emphasize experimenting with novel harness architectures, optimizing for performance and cost, and pushing the boundaries of agent capabilities.
- **Vibe Coders / Solo Developers**: Seek minimal, easy-to-setup harnesses that provide immediate benefits (e.g., a simple `CLAUDE.md` with a few skills and a test-verification loop).

## Failure Modes

- **Over-constraining**: Making the harness too rigid, preventing the agent from adapting to novel situations.
- **Under-constraining**: Insufficient safeguards leading to unsafe or incorrect outputs.
- **Complexity bloat**: Harnesses become difficult to maintain and debug.
- **Misaligned feedback loops**: Verification metrics that don't correlate with true task success.
- **Context drift**: Poor context management causing the agent to lose track of goals.

## When Harness Engineering is Unnecessary

For trivial, one-shot tasks where the model can succeed with a well-crafted prompt alone (e.g., simple text transformations, basic queries), a full harness may be overkill. However, as task complexity increases, harness engineering becomes essential.

## Claude Code Implementation

In Claude Code, the harness is configured through:

- **CLAUDE.md**: Project-specific instructions and context.
- **.claude/ directory**: Contains skills, agents, commands, and hooks.
- **MCP Servers**: For extending tool capabilities.
- **Hooks**: For running verification steps (e.g., pre-tool hooks for linting, post-tool hooks for testing).
- **Agent delegation**: Using subagents for specialized tasks.
- **Permissions**: Controlling tool access via the CLI or configuration.

Example harness elements in Claude Code:
- A skill that runs tests and returns results for a verification loop.
- A hook that automatically lints code after every edit.
- An agent that delegates research tasks to a subagent with web search capabilities.

## References

1. Martin Fowler: [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)
2. Addy Osmani: [Agent Harness Engineering](https://addyosmani.com/blog/agent-harness-engineering/)
3. LangChain: [The Anatomy of an Agent Harness](https://www.langchain.com/blog/the-anatomy-of-an-agent-harness)
4. OpenAI: [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
5. Wikipedia: [Agent harness](https://en.wikipedia.org/wiki/Agent_harness)

---