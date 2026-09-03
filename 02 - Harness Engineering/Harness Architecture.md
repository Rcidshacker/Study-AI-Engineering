---
title: Harness Architecture
aliases:
  - Agent Harness Structure
  - Harness Components Overview
tags:
  - harness-engineering
  - architecture
  - ai-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Harness Architecture

## What is a Harness?

In AI agent systems, the harness refers to everything outside the core language model that enables it to function as an agent. While the model provides reasoning capabilities, the harness provides the scaffolding, constraints, tools, and environment that turn raw model capabilities into purposeful agent behavior.

**Agent = Model + Harness**

The harness is what transforms a general-purpose language model into a specialized AI agent capable of performing specific tasks reliably and safely.

## Core Harness Components

A well-designed agent harness consists of several interconnected components:

### 1. Instructions and Constraints
**Defining what the agent should and should not do**

#### Elements:
- **System prompts**: High-level behavioral guidelines
- **Task descriptions**: Specific objectives and goals
- **Constraints**: Boundaries on behavior, format, or content
- **Principles**: Ethical guidelines and operational principles
- **Examples**: Few-shot demonstrations of desired behavior
- **Role definitions**: Specifying the agent's persona or expertise

#### Storage Locations:
- CLAUDE.md files (primary mechanism in Claude Code)
- Skill descriptions
- Agent definitions
- Hook implementations
- MCP server instructions

#### Best Practices:
- Be specific and unambiguous
- Prioritize critical constraints
- Use positive framing (what to do) over negative (what not to do)
- Include examples for complex expectations
- Regularly review and update based on performance

### 2. Tools and Permissions
**Defining what the agent can do and how**

#### Elements:
- **Available functions**: Specific capabilities the agent can invoke
- **Access scopes**: What resources each tool can access (files, networks, etc.)
- **Permission levels**: Read-only vs. read-write vs. administrative access
- **Rate limits**: Frequency constraints on tool usage
- **Tool descriptions**: Clear explanations of what each tool does
- **Parameter schemas**: Structured definitions of tool inputs and outputs

#### Tool Categories:
- **File operations**: Read, write, edit, search, delete files
- **Code execution**: Run scripts, compile programs, execute commands
- **Information retrieval**: Web search, database queries, API calls
- **Communication**: Send messages, emails, notifications
- **Transformation**: Format conversion, data processing, analysis
- **Verification**: Testing, linting, validation, quality checks

#### Best Practices:
- Principle of least privilege: Only grant necessary permissions
- Clear tool descriptions prevent misuse
- Input validation at tool boundaries
- Logging and auditing of tool usage
- Regular permission reviews

### 3. Context and Information Access
**Determining what information the agent can access**

#### Elements:
- **Context sources**: Files, databases, APIs, web content
- **Retrieval mechanisms**: How information is fetched and presented
- **Context windows**: Limits on how much information can be processed at once
- **Freshness guarantees**: How current the information is
- **Relevance filtering**: Ensuring context is pertinent to the task
- **Information formatting**: How data is structured for model consumption

#### Sources:
- Local file systems
- Remote APIs and web services
- Knowledge bases and documentation
- Conversation history
- Agent state and memory
- External data streams

#### Best Practices:
- Balance completeness with context window limits
- Prioritize relevant, timely information
- Implement effective retrieval strategies
- Format information for optimal model consumption
- Regularly prune outdated or irrelevant context

### 4. State and Memory
**Managing persistent information across interactions**

#### Elements:
- **Short-term state**: Working memory for current task
- **Session state**: Information persisting within a conversation
- **Long-term state**: Knowledge accumulated across sessions
- **Procedural state**: Skills, habits, and learned behaviors
- **Declarative state**: Facts, knowledge, and reference information
- **Episodic state**: Specific experiences and lessons learned

#### Storage Mechanisms:
- Files (Markdown, JSON, YAML)
- Databases (SQLite, PostgreSQL, Redis)
- Key-value stores
- Vector databases (for embeddings)
- Knowledge graphs
- Cloud storage solutions

#### Best Practices:
- Distinguish between different state types and lifetimes
- Implement appropriate persistence mechanisms
- Plan for state evolution and migration
- Ensure state consistency and integrity
- Provide mechanisms for state inspection and control

### 5. Feedback and Verification
**Knowing when the agent's work is correct and complete**

#### Elements:
- **Success criteria**: Clear definitions of what constitutes success
- **Verification mechanisms**: How to check if work meets criteria
- **Feedback loops**: Processes for incorporating feedback into future work
- **Quality metrics**: Quantitative measures of output quality
- **Error detection**: Identifying mistakes, hallucinations, or deviations
- **Validation processes**: External checks against standards or requirements

#### Verification Types:
- **Syntactic validation**: Checking format and structure
- **Semantic validation**: Verifying meaning and correctness
- **Functional validation**: Testing that things work as intended
- **Comparative validation**: Checking against references or baselines
- **Human validation**: Expert review and approval

#### Best Practices:
- Define success criteria upfront
- Automate verification where possible
- Create tight feedback loops for rapid improvement
- Balance verification thoroughness with efficiency
- Learn from verification failures to improve future performance

### 6. Orchestration and Control
**Managing how the agent operates over time**

#### Elements:
- **Execution models**: How the agent processes tasks (reactive, deliberative, etc.)
- **Loop mechanisms**: Iterative processes for improvement and refinement
- **Hierarchy and delegation**: How complex tasks are broken down
- **Concurrency control**: Managing simultaneous operations
- **Error handling**: Responding to failures and unexpected conditions
- **Resource management**: Controlling compute, time, and token usage
- **Lifecycle management**: Starting, pausing, resuming, and terminating agent operations

#### Control Mechanisms:
- **Planning systems**: Determining how to approach tasks
- **Scheduling**: Deciding when to perform actions
- **Monitoring**: Tracking agent state and performance
- **Intervention points**: Where humans or other systems can intercede
- **Escalation procedures**: Handling situations beyond agent capabilities

#### Best Practices:
- Start with simple execution models and add complexity as needed
- Build in observable checkpoints for monitoring
- Design graceful degradation for error conditions
- Implement resource limits to prevent runaway consumption
- Create clear lifecycle management procedures

## Harness Layers Conceptual Model

One way to visualize the harness is as layers of increasing specificity:

```
Layer 6: Task-Specific Customization
    │
    ├── Custom instructions for this particular task
    ├── Specialized tools needed only here
    └── Unique context sources relevant to this work
Layer 5: Project/Workshop Standards
    │
    ├── Team conventions and practices
    ├── Shared tool configurations
    ├── Project-specific knowledge base
    └── Common verification standards
Layer 4: Domain Expertise
    │
    ├── Field-specific knowledge and practices
    ├── Specialized terminology and concepts
    ├── Domain-appropriate tools and methods
    └── Regulatory and compliance requirements
Layer 3: Agent Type Templates
    │
    ├── Researcher agent configuration
    ├── Coder agent setup
    ├── Writer agent framework
    └── Analyst agent parameters
Layer 2: Universal Agent Capabilities
    │
    ├── Basic reasoning enhancement
    ├── Fundamental tool sets (file ops, basic search)
    ├── Core safety and constraint systems
    └── Standard interaction patterns
Layer 1: Model Foundation
    │
    └── The underlying language model itself
```

This model shows how harnesses can be built up from universal capabilities to highly specific task adaptations.

## Harness Design Principles

### 1. Separation of Concerns
Different harness aspects should be modular and independently adjustable:
- Instructions can change without altering tools
- Tool permissions can be updated without affecting context access
- State mechanisms can evolve independently of verification systems

### 2. Progressive Disclosure
Start simple and reveal complexity only when needed:
- Begin with minimal viable harness
- Add components as limitations are identified
- Enable advanced features through opt-in mechanisms
- Keep the learning curve shallow for new users

### 3. Explicit Boundaries
Make harness limitations clear and visible:
- Clearly document what the agent cannot do
- Provide explicit feedback when boundaries are approached
- Design fail-safe mechanisms for boundary violations
- Regularly test and validate boundary effectiveness

### 4. Tunability and Adaptation
Harnesses should be adjustable based on experience:
- Parameters that can be adjusted without reprogramming
- Feedback mechanisms that inform harness improvements
- A/B testing capabilities for harness variations
- Learning systems that suggest harness optimizations

### 5. Transparency and Insight
Users should be able to understand and inspect the harness:
- Clear documentation of all harness components
- Mechanisms to view current harness configuration
- Logging and auditing of harness operations
- Tools for harness debugging and troubleshooting

### 6. Safety by Design
Safety considerations should be foundational:
- Default to restrictive rather than permissive
- Implement defense-in-depth with multiple safety layers
- Design for graceful failure rather than catastrophic failure
- Include mechanisms for human oversight and intervention

## Harness Implementation in Claude Code

Claude Code provides specific mechanisms for implementing each harness component:

### Instructions and Constraints
- **CLAUDE.md files**: Primary vehicle for project-level instructions
- **Skill descriptions**: How skills present their purpose and usage
- **Agent definitions**: Configuration of subagent behavior
- **Hook specifications**: Instructions for pre/post-tool actions
- **Command definitions**: How slash commands are presented

### Tools and Permissions
- **Built-in tools**: File operations, web search, code execution
- **MCP servers**: Extensible tool system for custom capabilities
- **Tool descriptions**: Automatic generation from function signatures
- **Permission scopes**: File system access controls
- **Rate limiting**: Built-in protections against abuse

### Context and Information Access
- **Automatic context loading**: CLAUDE.md files from workspace hierarchy
- **File reading**: Direct inclusion of file contents in context
- **Web search results**: Search findings added to conversation context
- **Skill invocation**: Loading skill descriptions and examples
- **MCP responses**: External data integrated into context
- **State persistence**: claude-progress.md, DECISIONS.md

### State and Memory
- **Session state**: Conversation history and working context
- **Progress tracking**: claude-progress.md for task state
- **Decision logging**: DECISIONS.md for important choices
- **Skill state**: Skills can maintain their own state files
- **Knowledge base**: .claude/knowledge/ directory for shared information
- **User preferences**: Customizable agent behavior persistence

### Feedback and Verification
- **Post-tool hooks**: Automatic verification after actions
- **Pre-tool hooks**: Input validation before actions
- **User input hooks**: Pausing for human confirmation
- **Context enrichment hooks**: Adding verification information
- **Skill-based verification**: Custom skills for specific validation types
- **External MCP validators**: Third-party verification services

### Orchestration and Control
- **Subagent delegation**: Breaking work among specialized agents
- **Skill chaining**: Sequencing skills for complex workflows
- **Hook-driven automation**: Triggering actions based on events
- **Interactive mode**: Natural conversation for human-guided control
- **Iteration limits**: Built-in protections against infinite loops
- **Resource monitoring**: Token usage and context window management

## Evaluating Harness Effectiveness

### 1. Reliability Metrics
- **Success rate**: Percentage of tasks completed successfully
- **Consistency**: Similar performance across similar tasks
- **Error frequency**: How often mistakes occur
- **Recovery ability**: Capability to recover from mistakes
- **Predictability**: Ability to anticipate agent behavior

### 2. Capability Metrics
- **Tool utilization**: How effectively available tools are used
- **Information leverage**: How well context is used to improve outputs
- **Skill effectiveness**: How well specialized capabilities perform
- **Adaptability**: Ability to handle variations and edge cases
- **Learning rate**: Improvement over time with experience

### 3. Efficiency Metrics
- **Token efficiency**: Quality of output per token consumed
- **Time efficiency**: Results achieved per unit of time
- **Resource efficiency**: Compute and memory usage effectiveness
- **Cost efficiency**: Value obtained per unit of cost
- **Scalability**: Performance maintenance as task size increases

### 4. Safety Metrics
- **Boundary adherence**: Respect of defined constraints and limits
- **Harm prevention**: Avoidance of unsafe or undesirable outputs
- **Failure mode safety**: Graceful handling of error conditions
- **Intervention effectiveness**: How well human oversight works
- **Compliance**: Adherence to relevant regulations and standards

### 5. User Experience Metrics
- **Clarity of communication**: How well the agent explains itself
- **Responsiveness**: Speed and appropriateness of reactions
- **Personalization**: Adaptation to user preferences and style
- **Trustworthiness**: Degree to which users rely on the agent
- **Satisfaction**: Overall user happiness with agent performance

## Harness Evolution and Maturity Model

### Individual Agent Maturity
1. **Level 0**: Raw model access (no harness)
2. **Level 1**: Basic harness (minimal CLAUDE.md, basic file access)
3. **Level 2**: Functional harness (working tools, basic state)
4. **Level 3**: Reliable harness (verification loops, error handling)
5. **Level 4**: Adaptive harness (learning from failures, self-improvement)
6. **Level 5**: Optimized harness (performance tuned, minimal supervision)

### Team/Project Harness Maturity
1. **Level 0**: Ad-hoc, per-session configuration
2. **Level 1**: Shared basic CLAUDE.md and tools
3. **Level 2**: Standardized harness with version control
4. **Level 3**: Project-specific skills and MCP integrations
5. **Level 4**: Advanced state management and learning systems
6. **Level 5**: Full observability, compliance, and enterprise integration

### Enterprise Harness Maturity
1. **Level 0**: Individual experimentation only
2. **Level 1**: Department-level sharing of basic harnesses
3. **Level 2**: Organization-wide harness standards
4. **Level 3**: Custom MCP servers and enterprise integrations
5. **Level 4**: Advanced monitoring, auditability, and compliance
6. **Level 5**: AI agent platform with governance and lifecycle management

## Getting Started with Harness Engineering in Claude Code

### For Beginners
1. **Start with CLAUDE.md**: Create a simple file with 5-10 key guidelines
2. **Explore built-in tools**: Understand what file operations, web search, and code execution can do
3. **Add one useful skill**: Find or create a skill for a repetitive task you perform
4. **Implement basic verification**: Add a post-tool hook that runs a simple check after file edits
5. **Track progress**: Begin using claude-progress.md to remember where you left off

### For Intermediate Practitioners
1. **Develop custom skills**: Create skills that encapsulate your domain-specific workflows
2. **Set up MCP servers**: Integrate external services that provide valuable capabilities
3. **Implement comprehensive verification**: Build layered verification (syntax, semantics, functionality)
4. **Design state management**: Create systems for tracking long-term project knowledge
5. **Add observability**: Implement logging and metrics to understand harness performance

### For Advanced Engineers
1. **Design adaptive harnesses**: Build systems that self-optimize based on performance data
2. **Create meta-harnesses**: Harnesses that observe and improve other harnesses
3. **Implement distributed harness architectures**: For large-scale, multi-team agent systems
4. **Build specialized MCP servers**: For proprietary data sources or specialized capabilities
5. **Develop harness testing frameworks**: Systematic ways to evaluate harness effectiveness

## Relationship to Other Engineering Disciplines

### With Loop Engineering
- The harness provides the environment and constraints within which loops operate
- Loop mechanisms (verification, retry, improvement) are implemented using harness components
- Effective harness design makes loops more powerful and easier to implement correctly

### With Context Engineering
- Context engineering is a specialized subset of harness engineering focused on information management
- The harness determines what context sources are available and how they can be accessed
- Context engineering techniques enhance the harness's information access capabilities

### With Graph Engineering
- Harness engineering provides the foundation for individual agents in a graph
- Graph orchestration relies on harness-provided tools, state, and communication mechanisms
- Individual nodes in an agent graph execute using their respective harnesses

### With Agent Engineering
- Harness engineering is a core component of the Agent = Model + Harness equation
- Effective harness design determines what kinds of agents can be built
- Harness limitations fundamentally constrain agent capabilities and behavior

## References and Further Reading

- [[Agent Engineering]] - For the foundational concept of agents as model + harness
- [[Loop Engineering]] - For how loops operate within harness constraints
- [[Context Engineering]] - For specialized information management within the harness
- [[Agent State]] - For persistent information management as a harness component
- [[Agent Loops]] - For specific loop patterns that utilize harness components
- [[Agent Orchestration]] - For how multiple harnessed agents work together
- [[Claude Code Architecture]] - For specific harness implementation in Claude Code
- [[Learning Roadmap]] - For structured progression in harness engineering skills

---