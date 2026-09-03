---
title: Harness vs Loop vs Graph
aliases:
  - Three Engineering Paradigms Comparison
  - Agent System Layers Comparison
tags:
  - comparisons
  - harness-engineering
  - loop-engineering
  - graph-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Harness Engineering vs Loop Engineering vs Graph Engineering

This note compares and contrasts the three core engineering paradigms in AI agent systems, clarifying their distinct focuses, relationships, and when to apply each.

## Core Definitions

### Harness Engineering
**Focus**: The environment and constraints that enable an agent to function
**Question**: *What can the agent do, and under what conditions?*
**Analogy**: The scaffolding, safety systems, and operational boundaries of a construction site

### Loop Engineering
**Focus**: The iterative cycles through which an agent pursues goals
**Question**: *How does the agent improve and verify its work over time?*
**Analogy**: The plan-do-check-act (PDCA) cycle or scientific method applied to agent behavior

### Graph Engineering
**Focus**: The relationships, routing, and state transitions in complex agent systems
**Question**: *How is work divided, coordinated, and flowed between multiple agents or steps?*
**Analogy**: The organizational chart, workflow diagrams, and communication protocols of a company

## Detailed Comparison

| Aspect | Harness Engineering | Loop Engineering | Graph Engineering |
|--------|-------------------|------------------|-------------------|
| **Primary Concern** | Capabilities and constraints | Improvement through iteration | Coordination and relationships |
| **Key Question** | What can the agent do? | How does it get better? | How do agents work together? |
| **Time Scale** | Long-term (system setup) | Medium-term (task execution) | Short to medium-term (workflow steps) |
| **Abstraction Level** | System architecture | Behavioral patterns | Structural organization |
| **Stability** | Relatively static (changes infrequently) | Dynamic (changes during execution) | Can be static or dynamic |
| **Failure Modes** | Over/under-constraining, tool mismatches | Infinite loops, wrong success criteria | State inconsistency, deadlocks, complexity |
| **Success Metrics** | Reliability, safety, tool efficacy | Convergence rate, quality improvement | Throughput, coordination efficiency, fault tolerance |

## Relationships and Dependencies

### Harness Contains Loops
- The harness provides the environment in which loops operate
- Loop mechanisms (verification, retry) are implemented using harness components
- Harness engineering creates the conditions for effective looping

**Example**: 
- A verification loop (loop engineering) requires testing tools (harness engineering)
- State persistence for loop continuity depends on harness state management
- Tool availability for loop actions is determined by harness permissions

### Loops Can Form Graphs
- A single agent loop is the simplest possible graph (one node with a self-edge)
- Multiple interconnected loops create graph-like structures
- Loop termination conditions can determine graph transitions

**Example**:
- A test/fix loop (node) that repeats until tests pass (self-edge)
- Multiple test/fix loops for different components connected in a workflow graph
- Loop outcomes (pass/fail) determining whether to proceed to next workflow step

### Graphs Depend on Harness and Loops
- Graph execution relies on harness-provided tools and context
- Individual nodes in a graph typically execute loops
- Graph state management often uses harness state mechanisms

**Example**:
- Each agent in an orchestration graph runs its own internal loop
- Graph transitions may be triggered by loop completion signals
- Shared state in a graph may be persisted using harness mechanisms

## When to Focus on Each Paradigm

### Prioritize Harness Engineering When:
- Setting up a new agent system or project
- Agents consistently fail due to missing capabilities or context
- Need to establish basic reliability, safety, and tool access
- Defining agent boundaries and permissions
- Creating reusable components (skills, tools, instructions)

### Prioritize Loop Engineering When:
- Agents complete tasks but with inconsistent quality or reliability
- Need for verification, correction, and improvement mechanisms
- Want to reduce human supervision through self-correction
- Tasks benefit from iterative refinement and learning
- Building foundational agent competencies (research, coding, etc.)

### Prioritize Graph Engineering When:
- Tasks require multiple specialized roles or expertise
- Parallel execution would significantly improve efficiency
- Complex conditional logic or workflow routing is needed
- Multiple agents need to share and update common state
- Need for auditability, tracing, and human oversight at specific points
- Scaling beyond what a single agent loop can manage

## Claude Code Implementation Mapping

| Paradigm | Claude Code Manifestations | Implementation Examples |
|----------|----------------------------|-------------------------|
| **Harness Engineering** | • CLAUDE.md files<br>• Skills system<br>• Tools and MCP servers<br>• Environment setup<br>• State persistence files | • Comprehensive project CLAUDE.md<br>• Custom verification skill<br>• GitHub MCP server<br>• Reproducible devcontainer setup<br>• claude-progress.md and DECISIONS.md |
| **Loop Engineering** | • Built-in agentic loop (plan-act-observe-verify)<br>• Verification loops via hooks/skills<br>• Research loops<br>• The `/loop` skill<br>• Hook-driven cycles | • Post-edit hook running tests<br>• Research skill with exploration/synthesis/validation phases<br>• `/loop` skill for iterative improvement<br>• Pre-tool hook validating inputs<br>• Skill chaining for complex workflows |
| **Graph Engineering** | • Subagent delegation graphs<br>• Skill routing and selection<br>• Workflow graphs encoded in skills<br>• External graph orchestration via MCP<br>• State flow along graph edges | • Planner → Architect → Developer → Tester agent graph<br>• Skill matcher routing to appropriate capabilities<br>• Feature development workflow as a skill<br>• LangGraph/AutoGen integration via MCP<br>• Progress tracking along workflow steps |

## Anti-Patterns and Misapplications

### Harness Engineering Anti-Patterns
- **Over-constraining**: Too many restrictions prevent agent adaptability
  *Example*: CLAUDE.md with 500 lines of overly specific rules
- **Under-constraining**: Insufficient safeguards lead to unsafe behavior
  *Example*: No tool permissions or verification mechanisms
- **Tool Mismatch**: Providing wrong tools for the task
  *Example*: Giving file delete tools when only read access is needed
- **Context Starvation/Overflow**: Too little or too much information
  *Example*: Empty CLAUDE.md vs. 1000-line comprehensive manual

### Loop Engineering Anti-Patterns
- **Infinite Loops**: Missing or ineffective termination conditions
  *Example*: Test/fix loop with no max iteration limit
- **Wrong Success Criteria**: Verification doesn't match true goals
  *Example*: Optimizing for test coverage while missing functionality
- **Verification Bottlenecks**: Slow checks make loops impractical
  *Example*: Running full e2e test suite on every minor edit
- **Over-refinement**: Perfecting insignificant details
  *Example*: Spending iterations on code formatting while missing logic errors

### Graph Engineering Anti-Patterns
- **Over-engineering**: Using graphs for simple linear tasks
  *Example*: Complex orchestration for a two-step process
- **State Inconsistency**: Poor state management causes conflicts
  *Example*: Two agents updating same shared state without coordination
- **Deadlocks**: Circular dependencies halt progress
  *Example*: Agent A waits for B, B waits for A
- **Complexity Explosion**: Spending more time managing graph than doing work
  *Example*: Elaborate graph for a simple CRUD application

## Evolution and Maturity Model

### Individual Agent Maturity
1. **Level 0**: Raw model (no harness)
2. **Level 1**: Basic harness (minimal CLAUDE.md, basic tools)
3. **Level 2**: Functional harness (working tools, basic state)
4. **Level 3**: Reliable harness (verification loops, error handling)
5. **Level 4**: Adaptive harness (learning from failures, self-improvement)
6. **Level 5**: Optimized harness (performance tuned, minimal supervision)

### Multi-Agent System Maturity
1. **Level 0**: Single agent only
2. **Level 2**: Basic agent delegation (main agent → subagent)
3. **Level 3**: Structured workflows (defined handoffs, verification)
4. **Level 4**: Specialized roles (distinct agent types with clear responsibilities)
5. **Level 5**: Dynamic orchestration (adaptive graphs, learning coordination)

## Practical Guidelines by User Type

### For Vibe Coders / Solo Developers
1. **Harness**: Start with a simple CLAUDE.md (20-50 lines) covering core conventions
2. **Loops**: Add a basic verification loop (lint → test) using hooks or skills
3. **Graphs**: Usually unnecessary; use sequential agent work or simple skill chaining
4. **Focus**: Get reliable single-agent operation before considering complexity

### For Teams / Projects
1. **Harness**: Project-wide CLAUDE.md, shared skill library, standardized MCP setup
2. **Loops**: Team-wide verification standards, automated testing hooks, improvement loops
3. **Graphs**: Defined agent roles (planner, architect, coder, tester), clear handoff procedures
4. **Focus**: Balance between standardization and flexibility for team productivity

### For Production Systems / Enterprises
1. **Harness**: Comprehensive observability, security boundaries, compliance validation
2. **Loops**: Multi-layer verification, canary deployments, automated rollback, retrospective learning
3. **Graphs**: Explicit workflow definitions, audit trails, SLA monitoring, incident response graphs
4. **Focus**: Reliability, traceability, and integration with existing enterprise systems

## Decision Framework

Use this flowchart to determine where to focus your efforts:

```
Start
  │
  ▼
[Are agents consistently failing due to missing tools/context/permissions?]
  │ Yes
  ▼
[Focus on Harness Engineering]
  │
  ◄── No ──[Do agents complete tasks but with unreliable quality?]
          │ Yes
          ▼
      [Focus on Loop Engineering]
          │
          ◄── No ──[Does the task require multiple specialists or parallelism?]
                  │ Yes
                  ▼
              [Focus on Graph Engineering]
                  │
                  ◄── No ──[You likely have a well-balanced system!]
```

## Hybrid Approaches and Integration

### Harness + Loop Integration Patterns
- **Verification-Driven Development**: Harness provides verification, loops use it for improvement
- **Context-Managed Learning**: Harness manages context, loops refine understanding through iteration
- **Tool-Enhanced Iteration**: Harness provides specialized tools, loops apply them repeatedly for mastery
- **State-Powered Progress**: Harness persists state across sessions, loops build upon previous work

### Loop + Graph Integration Patterns
- **Nested Loops**: Inner loops (micro-corrections) within outer graph steps
- **Graph-Controlled Loops**: Graph structure determines when and how loops execute
- **Loop Results as Graph Transitions**: Loop outcomes determine which graph path to take
- **Parallel Loop Execution**: Multiple agents running similar loops in different graph branches

### Harness + Graph Integration Patterns
- **Graph-Defined Tool Access**: Graph state controls which harness tools are available at each step
- **Harness-Managed Graph Resources**: Harness provides compute/memory for graph execution nodes
- **State Synchronization**: Harness state mechanisms keep graph state consistent across executions
- **Observability Integration**: Harness monitoring tracks graph execution performance and health

## Final Synthesis

The three engineering paradigms are not competing alternatives but complementary layers that together form a complete approach to building effective AI agent systems:

- **Harness Engineering** creates the *stage* where agents can perform
- **Loop Engineering** defines how agents *improve their performance* on that stage
- **Graph Engineering** orchestrates *multiple agents or steps* working together on complex productions

Effective agent systems typically implement all three layers, with the depth and sophistication of each layer matching the complexity of the tasks being addressed. The key is to start simple, solve real problems, and incrementally add sophistication where it provides measurable benefits.

Remember: The goal is not to maximize engineering complexity but to maximize agent reliability and usefulness in achieving human goals.