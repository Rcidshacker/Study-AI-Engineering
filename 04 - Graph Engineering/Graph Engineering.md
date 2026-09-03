---
title: Graph Engineering
aliases:
  - Agent Graph
  - Workflow Graph
  - Skill Graph
tags:
  - ai-engineering
  - agent-engineering
  - graph-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Graph Engineering

## Definition

Graph engineering is the practice of designing and implementing explicit graph structures to organize complex agent systems, where nodes represent units of work (agents, tools, or steps) and edges represent transitions, dependencies, or communication flows between them. Unlike linear agent loops, graphs enable modeling of parallelism, conditional routing, shared state, and multi-agent coordination.

The core insight is that complex tasks often require heterogeneous expertise, interdependent subtasks, and persistent shared state that exceed the capabilities of a single agent running a simple loop.

## Origin and Popularization

The term "graph engineering" emerged in mid-2026 as a natural evolution beyond loop engineering. Key contributors and milestones include:

- **Teklens.ai article** (July 2026): Positioned graph engineering as the stage following prompt → context → harness → loop engineering, describing how several agents work together as an organization.
- **ArXiv paper** "Graph Engineering in the Era of LLM Agents: From Individual Intelligence to System Intelligence" (August 2026): Formal academic treatment introducing graph engineering as a paradigm for building next-generation agent systems.
- **Andrew Ng's work**: Demonstrated that wrapping GPT-3.5 in agentic workflows significantly improved performance (from 48.1% to 95.1% on HumanEval).
- **Sangam Pandey's blog** (July 2026): Emphasized the authoring step - deciding workflow control flow before agent execution by drawing graphs.
- **AI Builder Club discussions**: Clarified that graph engineering is a layer above loop engineering, with single loops being the simplest possible graphs.

## Core Components of Agent Graphs

A well-designed agent graph consists of:

1. **Nodes (Vertices)**
   - **Agent Nodes**: Individual AI agents with specific roles or capabilities.
   - **Tool Nodes**: Functions or MCP servers that perform specific actions.
   - **Workflow Steps**: Discrete units of work in a process.
   - **Skill Nodes**: Encapsulated capabilities that can be reused.
   - **Decision Points**: Nodes that evaluate conditions and route accordingly.

2. **Edges (Connections)**
   - **Sequential Edges**: Fixed transitions (A → B → C).
   - **Conditional Edges**: Branching based on state or outcomes (if success → C, else → D).
   - **Parallel Edges**: Fan-out/fan-in patterns (A → [B,C] → D).
   - **Loop Edges**: Cycles for repetition or feedback (A → B → A).
   - **Shared State Edges**: Connections that propagate context or data.

3. **State Management**
   - **Graph State**: Shared data structure accessible to nodes.
   - **Node State**: Private state within individual nodes.
   - **Checkpointing**: Persisting state for recovery and debugging.
   - **State Reducers**: Functions that define how state updates are applied.

4. **Control Mechanisms**
   - **Entry Points**: Where graph execution begins.
   - **Exit Points**: Conditions that terminate graph execution.
   - **Interrupt Handling**: Pausing/resuming graph execution.
   - **Human-in-the-Loop**: Points where humans can inspect/modify state.

## Types of Agent Graphs

### By Structure
- **State Graphs**: Nodes are functions that transform state; edges determine next node based on state.
- **Activity Graphs**: Nodes represent activities or tasks; edges show dependencies.
- **Data Flow Graphs**: Focus on how data moves between processing steps.
- **Control Flow Graphs**: Emphasize the sequence of execution and branching logic.

### By Function
- **Agent Orchestration Graphs**: Coordinate multiple agents working together.
- **Workflow Graphs**: Represent business or development processes.
- **Skill Graphs**: Map relationships between capabilities and when to use them.
- **Tool Graphs**: Show available tools and their prerequisites/side effects.
- **Knowledge Graphs**: Represent interconnected facts and concepts.
- **Dependency Graphs**: Illustrate relationships between components or tasks.

### By Complexity
- **Simple Graphs**: Linear sequences or basic loops.
- **Hierarchical Graphs**: Graphs containing subgraphs (recursive structure).
- **Dynamic Graphs**: Structure can change during execution.
- **Conditional Graphs**: Edges determined by runtime conditions.

## Importance Over Linear Loops

Graph engineering becomes valuable when:

1. **Parallelism is Beneficial**: Tasks can be split into independent subtasks executed concurrently.
2. **Specialization is Needed**: Different steps require different tools, models, or expertise.
3. **Complex Routing**: Workflow requires branching, merging, or conditional logic beyond simple loops.
4. **Persistent Shared State**: Multiple agents need to access and modify common information.
5. **Fault Tolerance**: Need checkpointing, recovery, and the ability to resume from specific points.
6. **Human Oversight**: Require structured points for human inspection and intervention.
7. **Scalability**: Systems need to grow beyond what a single agent loop can manage.

## Relationship to Other Engineering Paradigms

- **vs Loop Engineering**: A single agent loop is the simplest possible graph (one node with a self-edge). Graph engineering generalizes this to multiple nodes and complex routing.
- **vs Harness Engineering**: Graphs can be implemented as part of an agent's harness, particularly in the orchestration logic component.
- **vs Context Engineering**: Graphs often manage context through shared state flowing along edges.
- **Prompt Engineering**: Still relevant at the node level (what prompts each agent/node receives).

## Claude Code Implementation

While Claude Code primarily excels at single-agent loop engineering, graph engineering concepts can be implemented through:

1. **Subagent Delegation**
   - Main agent delegates to specialized subagents (different nodes).
   - Subagents return results to the main agent (edges).
   - Example: Research agent delegates to web search, summarization, and fact-checking subagents.

2. **Skills as Nodes**
   - Different skills represent different types of work.
   - The agent decides which skill to invoke based on context (routing logic).
   - Skills can call other skills (subgraphs).

3. **Hooks for Graph-Like Behavior**
   - Pre/post hooks can implement state transitions.
   - Example: A hook that checks test results and routes to either completion or fixing skills.

4. **MCP Servers for External Coordination**
   - Connecting to external orchestration systems (LangGraph, AutoGen) via MCP.
   - Claude Code acts as one node in a larger graph managed externally.

5. **CLAUDE.md for Graph Specifications**
   - Defining agent roles, responsibilities, and handoff procedures.
   - Example: Specifying that certain file types should be handled by specific subagents.

6. **State Persistence Mechanisms**
   - Using files or databases to maintain shared state between agent invocations.
   - Implementing checkpointing through periodic saves.

## Practical Examples

### Research Workflow Graph
```
[Research Planner] → [Web Searcher] → [Reader/Summarizer]
      ↓                 ↓             ↓
[Fact Checker] ← [Gap Identifier] → [Additional Search]
      ↓
[Synthesizer] → [Output Generator]
```

### Feature Development Graph
```
[Product Manager] → [Architect] → [Lead Developer]
      ↓                     ↓             ↓
[Frontend Dev] ← [API Designer] → [Backend Dev]
      ↓                     ↓             ↓
[QA Engineer] ← [Test Writer] ← [Code Reviewer]
      ↓
[Deployer] → [Monitor]
```

### Skill Routing Graph
```
[Task Analyzer] 
      ↓
[Skill Matcher] → {[Code Writer], [Debugger], [Tester], [Documenter]}
      ↓
[Result Integrator] → [Quality Checker] → {[Done], [Needs Revision]}
```

## Failure Modes

- **Over-Engineering**: Using graphs for simple tasks that would be better served by loops.
- **State Inconsistency**: Poor state management leading to conflicting updates.
- **Deadlocks**: Circular dependencies that prevent progress.
- **Complexity Explosion**: Graphs becoming too difficult to understand or maintain.
- **Performance Overhead**: Graph management costs outweighing benefits.
- **Incorrect Routing**: Misaligned conditional edges causing wrong workflow paths.
- **Checkpoint Failures**: Inability to recover state properly after interruption.

## When Graph Engineering is Overkill

For simple, linear tasks with clear sequential steps and minimal need for parallelism or specialization, a well-designed agent loop (possibly with nested loops) is often simpler and more effective than a graph implementation.

## Claude Code vs. Dedicated Graph Frameworks

While Claude Code can implement graph-like patterns through subagents and skills, dedicated frameworks like LangGraph, AutoGen GraphFlow, and Google ADK offer:

- Built-in state management and checkpointing.
- Visual graph design and execution monitoring.
- Advanced routing capabilities (conditional edges, fan-out/fan-in).
- Human-in-the-loop integration points.
- Better observability and debugging tools.

However, for many agentic coding tasks, Claude Code's built-in capabilities combined with thoughtful harness and loop engineering may suffice without needing explicit graph structures.

## References

1. Teklens.ai: [Graph Engineering: Multi-Agent Workflows, Not Chat Chains](https://teklens.ai/en/pm-lab/graph-engineering)
2. ArXiv: [Graph Engineering in the Era of LLM Agents: From Individual Intelligence to System Intelligence](https://arxiv.org/pdf/2608.21156v2)
3. AI Builder Club: [Is Graph Engineering Just LangGraph? LangGraph vs ...](https://www.aibuilderclub.com/blog/is-graph-engineering-just-langgraph)
4. Microsoft AutoGen: [Using LangGraph-Backed Agent](https://microsoft.github.io/autogen/0.4.4/user-guide/core-user-guide/cookbook/langgraph-agent.html)
5. LangChain: [LangGraph Documentation](https://docs.langchain.com/oss/javascript/langgraph/workflows-agents)
6. Google ADK: Agent Development Kit documentation
7. Sangam Pandey: [Graph Engineering: When an Agent Loop Should Be a Graph](https://sangampandey.info/blog/graph-engineering-agent-loops-to-graphs)

---