---
title: Glossary
aliases:
  - Terminology Reference
  - Key Terms Definition
tags:
  - moc
  - glossary
  - terminology
  - ai-engineering
  - agent-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Glossary of Key Terms

This glossary defines important terms used throughout the AI engineering knowledge base, particularly related to harness engineering, loop engineering, and graph engineering.

## A

**Agent**: An AI system that combines a language model with a harness (scaffolding) to perceive its environment, take actions, and achieve goals. Formula: Agent = Model + Harness.

**Agent Loop**: The iterative cycle that an agent executes to achieve goals, typically involving planning, execution, observation, verification, and correction.

**Agent Harness**: Everything outside the model weights that enables an AI model to operate effectively as an agent, including instructions, tools, environment, state, feedback mechanisms, and orchestration logic.

**Agent Orchestration**: The coordination and management of multiple agents working together to achieve complex goals that exceed the capabilities of a single agent.

**Agent State**: The information that an agent maintains about itself and its environment across time, including short-term context and long-term memory.

**Agent Workflow**: A defined sequence of steps or operations that an agent follows to accomplish a specific task or goal.

**Algorithmic Loop**: A loop where the control flow is determined by predefined algorithms rather than agent decision-making.

## B

**Branching Logic**: The decision-making component of a loop or graph that determines what happens next based on current state, outcomes, or conditions.

**Build-Verify Loop**: A specific type of agent loop focused on building software and verifying its correctness through testing and validation.

## C

**Context Engineering**: The practice of managing and optimizing the information provided to a language model, including retrieval, summarization, and relevance filtering.

**Context Window**: The maximum amount of text (measured in tokens) that a language model can process at one time.

**Continuous Improvement Loop**: An agent loop designed to gradually enhance performance over time through reflection, learning, and adaptation.

**Control Flow Graph**: A representation of a program or workflow where nodes represent operations and edges represent possible execution paths.

**Cross-session Persistence**: The ability to retain and access information across different agent invocations or sessions.

## D

**Deterministic Loop**: A loop that follows a predictable, repeatable path given the same initial conditions and inputs.

**Dependency Graph**: A graph that illustrates the relationships between components or tasks, showing which items depend on others.

**Done Criterion**: A clear, measurable definition of when a task or goal has been successfully completed.

**Dynamic Graph**: A graph whose structure (nodes, edges, or both) can change during execution based on runtime conditions or state.

## E

**Edge**: In graph theory, a connection between two nodes that represents a relationship, transition, or flow.

**End-to-End (E2E) Verification**: Comprehensive testing that validates a complete user journey or workflow from start to finish.

**Evaluator-Optimizer Loop**: A loop pattern where one component generates output (optimizer) and another evaluates it (evaluator), with the optimizer refining its output based on evaluation feedback.

**Execution Loop**: The core cycle where an agent performs actions, observes results, and decides on subsequent actions.

**Exit Condition**: The specific criteria that cause a loop to terminate.

## F

**Feedback Loop**: A system where outputs are routed back as inputs to influence future behavior, enabling learning and adaptation.

**Feature List**: A machine-readable tracking system for work items, typically containing behavior descriptions, verification methods, and state information.

**Fixed Point Loop**: A loop that continues until the output stops changing significantly between iterations, indicating convergence.

**Flow Engineering**: The practice of designing and optimizing how work moves through a system, often using graph-based representations.

**Fork-Join Pattern**: A graph pattern where execution splits into parallel paths (fork) and later reconverges (join).

## G

**Gate**: A verification checkpoint that must be passed before proceeding to the next stage in a workflow or loop.

**Goal Definition**: A clear statement of what an agent is trying to achieve, including success criteria and completion conditions.

**Graph Engineering**: The practice of designing and implementing explicit graph structures to organize complex agent systems, where nodes represent units of work and edges represent transitions or relationships.

**Graph State**: The shared data structure that flows along the edges of an agent graph, accessible to nodes for reading and updating.

**Guardrail**: A safety mechanism or constraint that prevents an agent from performing unsafe or undesirable actions.

## H

**Harness Engineering**: The practice of designing and building the scaffolding (harness) around an AI model to enable it to operate effectively as an agent.

**Human-in-the-Loop (HITL)**: A system design that incorporates human judgment, intervention, or oversight at specific points in an automated workflow or agent loop.

**Hybrid Loop**: A loop that combines automated agent behavior with periodic human input or oversight.

## I

**Incremental Improvement Loop**: A loop designed to make small, continuous improvements rather than seeking dramatic changes in single iterations.

**Inner Loop**: A tight, fast cycle focused on immediate corrections or micro-improvements, often nested within an outer loop.

**Instruction Set**: The collection of rules, guidelines, and information that guides an agent's behavior, typically found in files like CLAUDE.md or AGENTS.md.

**Instrumentation**: The addition of monitoring, logging, or measurement capabilities to a system to observe its behavior and performance.

**Isolated Context**: A separate execution environment that prevents an agent's actions or state from affecting or being affected by other agents or processes.

## K

**Knowledge Graph**: A graph-based representation of knowledge where nodes represent entities or concepts and edges represent relationships between them.

**Loop Engineering**: The practice of designing and optimizing the iterative cycles (loops) that AI agents execute to achieve goals.

## L

**Layered Verification**: A verification strategy that employs multiple levels of checking with increasing thoroughness and cost, where each layer must pass before proceeding to the next.

**Learning Loop**: An agent loop specifically designed to acquire new knowledge or skills through experience and reflection.

**Lifecycle Event**: A specific point in an agent's execution (such as before/after tool use or session start/end) where automated actions can be triggered.

## M

**Maker-Checker Principle**: The concept that the same entity that creates something should not be solely responsible for verifying its correctness; independent verification is preferred.

**Memory System**: The components and mechanisms that enable an agent to store, retrieve, and utilize information across time.

**Meta-loop**: A loop that oversees or controls the execution of other loops, often used for system-level adaptation or improvement.

**Micro-loop**: A very tight, fast loop focused on immediate, small-scale corrections or validations.

**Model Context Protocol (MCP)**: A standardized protocol for connecting language models to external tools, services, and data sources.

**Multi-agent System**: A system composed of multiple interacting agents that work together to achieve individual or collective goals.

## N

**Node**: In graph theory, a point or vertex that represents an entity, agent, tool, step, or unit of work.

**Non-deterministic Loop**: A loop where the execution path can vary even with identical inputs due to randomness or uncertainty in agent behavior.

**Nested Loop**: A loop contained within another loop, allowing for hierarchical control structures.

## O

**Observability**: The ability to understand the internal state of a system by examining its external outputs, typically through logging, metrics, and tracing.

**Orchestrator-Worker Pattern**: A graph pattern where a central orchestrator agent delegates work to specialized worker agents and combines their results.

**Outer Loop**: A broader cycle that encompasses inner loops, often managing higher-level goals or workflows.

**Outcome Verification**: The process of checking whether the results of an agent's actions meet the desired goals or success criteria.

## P

**Parallel Execution**: The simultaneous execution of multiple independent processes or agents to improve efficiency.

**Persistence Mechanism**: A method for storing and retrieving information across agent sessions or system restarts.

**Policy Engine**: A component that enforces rules, constraints, and behavioral guidelines for agent behavior.

**Post-condition**: A condition that must be true after the execution of a specific action or loop iteration.

**Pre-condition**: A condition that must be true before the execution of a specific action or loop iteration.

**Prompt Engineering**: The practice of crafting effective inputs (prompts) to language models to elicit desired behaviors or outputs.

## Q

**Quality Gate**: A verification checkpoint that assesses whether work meets predefined quality standards before allowing progression.

**Query Loop**: A loop focused on iteratively refining questions, searches, or information gathering to improve results.

## R

**Re-planning Loop**: A loop where the agent periodically revises its plan based on new information, changing circumstances, or feedback from execution.

**Recursive Loop**: A loop that contains a call to itself, enabling self-similar patterns at different scales.

**Retry Loop**: A loop that automatically attempts an operation again after a failure, often with modifications or backoff strategies.

**Root Cause Analysis Loop**: A loop designed to iteratively investigate problems to identify underlying causes rather than just addressing symptoms.

**Routing Logic**: The decision-making component that determines which path to take in a graph based on current state, inputs, or conditions.

## S

**Safety Loop**: A loop specifically designed to monitor for and prevent unsafe or harmful behaviors.

**State Machine**: A mathematical model of computation used to design sequential logic systems, where the system can be in exactly one of a finite number of states at any given time.

**Stopping Condition**: The criteria that determine when a loop should terminate execution.

**Subagent**: A specialized agent created for a specific task or role, typically operating in isolated context and reporting results to a parent agent.

**Subgraph**: A portion of a graph that consists of a subset of nodes and edges from the original graph.

**Supervisor Loop**: A loop that monitors and manages the execution of other agents or loops, providing oversight and intervention capabilities.

**System Prompt**: The core set of instructions that defines a language model's fundamental behavior, capabilities, and constraints when operating as an agent.

## T

**Task Decomposition**: The process of breaking down a complex goal into smaller, more manageable subtasks or components.

**Task Graph**: A graph representation of a workflow where nodes represent tasks and edges represent dependencies or execution order.

**Testing Loop**: A loop focused on creating, executing, and analyzing tests to verify correctness and improve quality.

**Token**: The basic unit of text that language models process, typically representing words, parts of words, or punctuation.

**Traceability**: The ability to trace the origin, transformation, and usage of information or decisions through a system.

**Trigger Condition**: The specific event or state that initiates the execution of a loop, skill, or agent behavior.

## V

**Validation Loop**: A loop specifically designed to check whether outputs, processes, or systems meet predefined criteria or requirements.

**Verification Loop**: A loop focused on confirming that work meets success criteria through testing, checking, or other validation methods.

**Vertical Slice**: A narrow but complete implementation of a system that touches all layers or components, used to verify end-to-end functionality.

**Virtual Agent**: An agent implemented primarily through software configurations and prompts rather than custom code.

## W

**Workloop**: A colloquial term for an agent loop focused on accomplishing work tasks.

**Workflow Engine**: A system that executes predefined workflows, often using graph-based representations to define steps and dependencies.

**Working Context**: The information currently available to an agent for reasoning and decision-making, typically limited by the model's context window.

## Z

**Zero-shot Prompting**: Providing a prompt to a language model without any examples or demonstrations, relying solely on the model's innate capabilities.

---

## Term Relationships

### Engineering Paradigms
- **Harness Engineering** → Creates the environment and capabilities for agents
- **Loop Engineering** → Defines how agents improve through iteration
- **Graph Engineering** → Determines how complex agent systems are organized and coordinated

### Claude Code Specifics
- **CLAUDE.md** → Primary location for harness engineering instructions
- **Skills** → Modular units of harness components (tools, workflows, knowledge)
- **Hooks** → Mechanisms for implementing loop engineering through automation
- **Subagents** → Building blocks for graph engineering through specialization and delegation
- **MCP** → Extension mechanism for enhancing harness capabilities with external tools

### Common Patterns
- **Test/Fix Loop** → Combines loop engineering (iteration) with harness engineering (verification tools)
- **Skill Routing Graph** → Combines graph engineering (routing) with harness engineering (skills as nodes)
- **Subagent Delegation Graph** → Combines graph engineering (orchestration) with harness engineering (specialized agents)
- **Research Loop** → Combines loop engineering (iteration) with harness engineering (research tools and context management)

## Usage Notes

1. **Context Matters**: Many terms have slightly different meanings depending on the engineering paradigm or specific implementation context.
2. **Evolving Terminology**: As AI engineering is a rapidly evolving field, terminology may vary between sources and continue to develop.
3. **Overlapping Concepts**: Some terms appear in multiple paradigms because the concepts are interconnected (e.g., "state" is relevant to harness, loop, and graph engineering).
4. **Practical Focus**: Definitions emphasize practical usage and implementation over purely theoretical formulations.

## Further Reading

For deeper exploration of these terms, consult:
- The specific engineering notes ([[Harness Engineering]], [[Loop Engineering]], [[Graph Engineering]])
- Implementation guides ([[Claude Code Implementation Notes]])
- Scenario applications (in [[09 - Scenarios]])
- Repository analyses (in [[06 - GitHub Repositories]])

---