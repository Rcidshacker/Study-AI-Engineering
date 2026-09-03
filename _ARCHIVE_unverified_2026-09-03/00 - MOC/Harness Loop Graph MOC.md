---
title: Harness Loop Graph MOC
aliases:
  - Three Engineering Paradigms
  - Agent System Layers
tags:
  - moc
  - harness-engineering
  - loop-engineering
  - graph-engineering
  - ai-engineering
  - agent-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Harness Loop Graph MOC

This map explores the relationship between the three core engineering paradigms in AI agent systems: harness engineering, loop engineering, and graph engineering.

## The Three Layers Model

### [[Harness Engineering]]
The **foundation** - controls the environment and constraints
- What the agent can do (tools, permissions)
- What the agent knows (instructions, context)
- How the agent operates (environment, orchestration)
- Safety and reliability mechanisms
- State persistence and memory
- Observability and monitoring

### [[Loop Engineering]]
The **execution cycle** - controls repeated execution and feedback
- How the agent pursues goals (plan-act-observe-verify)
- Feedback mechanisms for improvement
- Termination conditions and stopping criteria
- Error handling and recovery
- Adaptation and learning from experience
- Human-in-the-loop integration

### [[Graph Engineering]]
The **coordination layer** - controls relationships, routing and state transitions
- How work is divided and coordinated
- Dependencies between tasks and agents
- Parallelism and specialization
- State sharing and communication
- Workflow orchestration and routing
- System-level architecture and patterns

## Relationships and Overlaps

### Harness Contains Loops
- The harness provides the environment in which loops operate
- Loop mechanisms (verification, retry) are components of the harness
- Harness engineering creates the conditions for effective looping

### Loops Can Form Graphs
- A single agent loop is the simplest graph (one node with self-edge)
- Multiple interconnected loops form graph structures
- Loop outcomes can determine graph transitions (conditional edges)

### Graphs Operate Within Harnesses
- Graph execution relies on harness-provided tools and context
- Harness manages resources for graph execution (compute, memory)
- Graph state persistence often uses harness state mechanisms

## Claude Code Implementation

### In Claude Code:
- **Harness**: CLAUDE.md, skills, tools, MCP, environment setup
- **Loops**: Built-in agentic loop, verification loops, `/loop` skill, hook-driven cycles
- **Graphs**: Subagent delegation graphs, skill routing graphs, workflow graphs in skills

### Example Relationships:
1. A CLAUDE.md file (harness) defines verification criteria
2. A hook (harness) runs tests after edits (loop component)
3. The test results determine whether to continue or fix (loop logic)
4. Different failure types route to different specialist agents (graph routing)
5. Specialist agents return results to main agent (graph convergence)

## Evolution of Agent Systems

### Historical Progression
1. **Prompt Engineering**: Focus on single-shot inputs
2. **Context Engineering**: Improve information provided to model
3. **Harness Engineering**: Build complete agent environment
4. **Loop Engineering**: Enable iterative improvement and verification
5. **Graph Engineering**: Coordinate multiple agents and complex workflows

### Current Understanding
- These are layers rather than strict stages
- Effective agent systems typically use all three layers
- The appropriate depth depends on task complexity
- Simpler tasks may only need harness + basic loops
- Complex systems benefit from explicit graph orchestration

## Decision Framework

### When to Focus on Each Layer

#### Start with Harness Engineering when:
- Setting up a new project or agent system
- Agents consistently fail due to missing tools or context
- Need to establish basic reliability and safety
- Defining agent capabilities and boundaries

#### Add Loop Engineering when:
- Agents complete tasks but with inconsistent quality
- Need for verification and correction mechanisms
- Want to reduce supervision through self-correction
- Tasks benefit from iterative refinement

#### Implement Graph Engineering when:
- Tasks require multiple specialized roles or expertise
- Parallel execution would significantly improve efficiency
- Complex conditional logic or workflow routing is needed
- Multiple agents need to share and update common state
- Need for auditability and human oversight at specific points

## Anti-Patterns and Failure Modes

### Harness Engineering Failures
- Over-constraining: Too many restrictions prevent agent adaptability
- Under-constraining: Insufficient safeguards lead to unsafe behavior
- Misaligned tools: Providing wrong tools for the task
- Poor context management: Information overload or starvation

### Loop Engineering Failures
- Infinite loops: Missing or ineffective termination conditions
- Wrong success criteria: Verification doesn't match true goals
- Verification bottlenecks: Slow checks make loops impractical
- Over-optimization: Perfecting insignificant details

### Graph Engineering Failures
- Over-engineering: Using graphs for simple linear tasks
- State inconsistency: Poor state management causes conflicts
- Deadlocks: Circular dependencies halt progress
- Complexity explosion: Graphs become unmaintainable

## Practical Guidelines

### For Individual Developers / Vibe Coders
1. **Harness**: Basic CLAUDE.md + essential skills
2. **Loops**: Simple verification loops (test/fix) via hooks
3. **Graphs**: Usually unnecessary; use sequential agent work

### For Teams / Projects
1. **Harness**: Project-wide CLAUDE.md, shared skills, standardized MCP
2. **Loops**: Team-wide verification standards, automated testing hooks
3. **Graphs**: Defined agent roles, clear handoff procedures, shared state

### For Production Systems
1. **Harness**: Comprehensive observability, security, and compliance
2. **Loops**: Multi-layer verification, canary deployments, rollback mechanisms
3. **Graphs**: Audit trails, explicit workflow definitions, SLA monitoring

## References and Further Reading

- [[Harness Engineering]]: Detailed exploration of agent scaffolding
- [[Loop Engineering]]: Deep dive on iterative improvement cycles
- [[Graph Engineering]]: Comprehensive guide to agent coordination
- [[Claude Code Architecture]]: How these concepts manifest in Claude Code
- [[Coding Agent Harness Example]]: Practical implementation of all three layers

---