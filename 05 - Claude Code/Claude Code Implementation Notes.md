---
title: Claude Code Implementation Notes
aliases:
  - Claude Code Harness Engineering
  - Claude Code Loop Engineering
  - Claude Code Graph Engineering
tags:
  - claude-code
  - harness-engineering
  - loop-engineering
  - graph-engineering
  - implementation
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Claude Code Implementation Notes

This document provides detailed implementation notes for applying harness engineering, loop engineering, and graph engineering concepts specifically within Claude Code.

## Harness Engineering in Claude Code

### 1. Instructions Subsystem

#### CLAUDE.md Optimization
- **Structure**: Router pattern with topic-specific references
- **Best Practices**:
  - Keep under 200 lines
  - Critical constraints at top and bottom (avoid "lost in the middle")
  - Use `@import` for modular organization (though Claude Code doesn't follow transitive imports)
  - Reference rather than duplicate information

#### AGENTS.md vs CLAUDE.md
- **CLAUDE.md**: Project-wide conventions, always-on context
- **AGENTS.md**: Agent-specific instructions, higher priority when present
- **Usage Pattern**: 
  - CLAUDE.md for general project conventions
  - AGENTS.md for specific agent behaviors or overrides

#### Topic Documentation
- Create focused documentation files (50-150 lines each)
- Place next to the code they govern
- Reference from CLAUDE.md/AGENTS.md
- Examples: API conventions, testing patterns, security guidelines

### 2. Tools Subsystem

#### Built-in Tools Optimization
- **File Operations**: Use relative paths, leverage glob patterns
- **Web Search**: Enable when current information is needed
- **Code Execution**: Prefer specific commands over vague requests
- **Browser Automation**: Use Interceptor skill for verification-heavy tasks

#### Custom Tools via Skills
- Encapsulate complex tool usage in skills
- Example: Database query skill that handles connection, execution, and result formatting
- Example: API interaction skill that manages authentication, retries, and error handling

#### MCP Servers
- **Filesystem MCP**: Standard file operations with enhanced capabilities
- **Git MCP**: Git operations with repository context
- **GitHub MCP**: Issue/PR management, code review integration
- **Database MCPs**: Direct database access for data-heavy tasks
- **Custom MCPs**: For proprietary systems or specialized APIs

### 3. Environment Subsystem

#### Reproducible Setup
- **init.sh/template**: Single command to reach executable state
- **Lockfiles**: `package-lock.json`, `poetry.lock`, `requirements.txt`
- **Version Pinning**: `.nvmrc`, `.python-version`, `Dockerfile`
- **Templates**: Use project templates over empty directories

#### Verification of Environment
- Build validation as part of setup
- Dependency conflict detection
- Runtime compatibility checks

### 4. State Subsystem

#### Cross-Session Persistence
- **claude-progress.md**: Current verified state, done/in-progress/blocked tracking
- **DECISIONS.md**: Architectural decisions with rationale (survives compaction)
- **Feature Lists**: Machine-readable progress tracking
- **Git Commits**: Atomic units as checkpoints

#### State Management Patterns
- **Append-only logs**: For audit trails and reproducibility
- **Structured data**: JSON/YAML for machine readability
- **Atomic updates**: Prevent corruption during concurrent access
- **Backup strategies**: Regular state snapshots

#### Compaction vs Reset Tradeoff
- **Compaction**: Preserves recent context, summarizes older content
- **Reset**: Complete context refresh, loses conversation history
- **Decision Framework**: 
  - Use compaction for continuous work on same topic
  - Use reset when context becomes misleading or irrelevant

### 5. Feedback Subsystem

#### Three-Layer Verification
1. **Layer 1**: Linting + Typechecking (fast, frequent)
2. **Layer 2**: Unit + Integration Tests (moderate speed, coverage)
3. **Layer 3**: End-to-End User Flows (slow, comprehensive)

#### Maker-Checker Separation
- **Principle**: Same model that wrote code cannot reliably judge it
- **Implementation**: 
  - Different prompts for implementation vs verification
  - Different models (if available) for checking
  - External verification systems via MCP
  - Human-in-the-loop for critical decisions

#### Verification Gate Design
- **Blocking Behavior**: Each gate must block progression to next
- **Failure Analysis**: Clear reporting of what failed and why
- **Retry Logic**: Automatic retry with exponential backoff for flaky tests
- **Escalation Paths**: Human intervention after N automatic failures

### 6. Orchestration Logic

#### Subagent Delegation
- **Isolated Context**: Each subagent gets fresh context window
- **Specialization**: Different subagents for different roles
- **Communication**: Structured handoffs via task descriptions
- **Result Aggregation**: Main agent synthesizes subagent outputs

#### Hook-Based Automation
- **Lifecycle Events**: Pre-tool, post-tool, session start/end
- **Deterministic Actions**: Linting, formatting, security checks
- **Conditional Triggers**: Based on file types, content patterns
- **External Integrations**: HTTP hooks to CI/CD, MCP hooks to external services

#### Workflow Management
- **WIP=1 Enforcement**: One active feature at a time
- **Feature Primitives**: Machine-readable `(behavior, verification, state)` triples
- **Progress Tracking**: Automated updates to tracking files
- **Blocking Dependencies**: Explicit dependency management

## Loop Engineering in Claude Code

### 1. Built-in Agentic Loop

#### Core Cycle
1. **Understand**: Parse request and context
2. **Plan**: Determine approach and tool selection
3. **Execute**: Use tools to perform actions
4. **Observe**: See results of actions
5. **Verify**: Check against criteria (via built-in or custom verification)
6. **Iterate**: Continue, adjust, or terminate based on verification

#### Customization Points
- **System Prompt**: Alters fundamental behavior and capabilities
- **CLAUDE.md**: Adds project-specific constraints and guidance
- **Skills**: Provide domain-specific knowledge and procedures
- **Hooks**: Intercept and modify execution at key points

### 2. Verification Loops

#### Test/Fix Loop Implementation
- **Trigger**: Failed test or verification step
- **Analysis**: Root cause identification
- **Correction**: Minimal change to fix issue
- **Re-verification**: Repeat until pass
- **Termination**: Max iterations to prevent infinite loops

#### Linting Loop
- **Continuous**: Run on every file edit
- **Automatic Fixing**: Auto-fix where possible (Prettier, ESLint --fix)
- **Education**: Explain violations to prevent recurrence
- **Escalation**: Manual intervention for complex issues

#### Type Checking Loop
- **Preventive**: Run before runtime execution
- **Incremental**: Check only changed files when possible
- **Explain Errors**: Provide context for type mismatches
- **Migration Assistance**: Help with type definition updates

### 3. Research Loops

#### Exploration Phase
- **Broad Search**: Initial wide-net information gathering
- **Source Evaluation**: Authority, relevance, recency assessment
- **Gap Identification**: What's missing, contradictory, or unclear

#### Deep Dive Phase
- **Focused Reading**: Deep engagement with key sources
- **Structured Note-taking**: Citations, connections, questions
- **Contradiction Resolution**: Evidence weighting and synthesis

#### Synthesis Phase
- **Cross-verification**: Fact-checking across multiple sources
- **Narrative Construction**: Building coherent explanation
- **Confidence Assessment**: Uncertainty quantification

#### Validation Phase
- **Peer Review**: Expert or knowledgeable agent feedback
- **Criteria Check**: Against original success criteria
- **Output Preparation**: Final deliverable with sources

### 4. Micro-Loops

#### Edit-Review Cycles
- **Immediate Feedback**: Syntax highlighting, basic validation
- **Iterative Refinement**: Small improvements based on feedback
- **Quality Thresholds**: Minimum standards before proceeding

#### Build-Test Cycles
- **Fast Feedback**: Compile/test cycle for immediate validation
- **Caching**: Skip unchanged components
- **Parallelization**: Run independent tests simultaneously

#### Design-Implementation Cycles
- **Prototyping**: Quick implementation to validate approach
- **Feedback Integration**: Adjust design based on implementation insights
- **Iterative Refinement**: Multiple passes to improve solution

### 5. Human-in-the-Loop Patterns

#### Review Points
- **Pre-implementation**: Design approval before coding
- **Mid-implementation**: Architecture check during complex work
- **Post-implementation**: Code review before merging
- **Pre-release**: Final verification before deployment

#### Intervention Triggers
- **Verification Failure**: After N automatic retry attempts
- **Uncertainty Detection**: Low confidence in approach or outcome
- **Ethical Concerns**: Potential safety or bias issues
- **Resource Exceedance**: Token/time/budget limits approached

#### Feedback Mechanisms
- **Structured Input**: Specific questions for human guidance
- **Context Provision**: Relevant information for informed decisions
- **Option Presentation**: Clear alternatives with tradeoffs
- **Decision Recording**: Capturing rationale for future reference

## Graph Engineering in Claude Code

### 1. Subagent Delegation Graphs

#### Orchestrator-Worker Pattern
```
[Main Agent]
      ↓
[Planner] → [Specialized Agent 1] 
      ↓                     ↓
[Specialized Agent 2] ← [Specialized Agent 3] → [Integrator]
      ↓
[Reviewer] → [Finalizer]
```

#### Implementation in Claude Code
- **Main Agent**: Breaks work, delegates to subagents
- **Specialized Agents**: Isolated work on specific aspects
- **Integrator**: Combines subagent outputs
- **Reviewer**: Quality check on integrated result
- **Communication**: Via task descriptions and result summaries

#### Dynamic Worker Creation
- **On-demand Specialization**: Create agents for specific subtasks
- **Skill-based Delegation**: Agents with pre-loaded relevant skills
- **Context Isolation**: Prevent contamination between workers
- **Result Aggregation**: Main synthesizer combines outputs

### 2. Skill Routing Graphs

#### Context-Based Skill Selection
```
[Task Analysis] 
      ↓
[Skill Matcher] → {[Code Writer], [Debugger], [Tester], [Documenter]}
      ↓
[Result Integrator] → [Quality Checker] → {[Done], [Needs Revision]}
```

#### Implementation in Claude Code
- **Skill Discovery**: Automatic based on description matching
- **Frontmatter Triggers**: When to invoke each skill
- **Skill Chaining**: Skills that invoke other skills
- **Fallback Mechanisms**: Alternative skills when primary fails
- **Performance Optimization**: Cache skill invocation results

#### Skill Graph Maintenance
- **Dependency Tracking**: Which skills depend on others
- **Conflict Resolution**: When multiple skills could apply
- **Version Management**: Skill updates and backward compatibility
- **Deprecation Paths**: Graceful removal of obsolete skills

### 3. Workflow Graphs

#### Feature Development Workflow
```
[Requirements] → [Design] → [Implementation] → [Verification] → [Review] → [Release]
      ↑                                         ↓
      └─────────────────────[Feedback]──────────┘
```

#### Implementation in Claude Code
- **Skills as Nodes**: Each major phase as a skill or agent
- **Hooks as Edges**: Automatic transitions between phases
- **State Persistence**: Progress tracking along the workflow
- **Conditional Routing**: Different paths based on intermediate results
- **Parallel Execution**: Independent phases running concurrently

#### State Flow in Workflows
- **Forward Flow**: Requirements → Design → Implementation → etc.
- **Feedback Loops**: Implementation results inform Design adjustments
- **Cross-cutting Concerns**: Security, performance, usability considerations
- **Milestone Tracking**: Verifiable completion of major phases

### 4. External Graph Orchestration

#### MCP-connected Graph Frameworks
- **LangGraph Integration**: Via MCP server exposing graph execution
- **AutoGen GraphFlow**: Conversational agent coordination via MCP
- **Custom Orchestrators**: Domain-specific workflow engines

#### Implementation Approach
- **Claude Code as Node**: One agent in a larger graph
- **MCP Tool Calls**: Trigger graph execution steps
- **State Exchange**: Graph state via MCP tool parameters/results
- **Monitoring & Control**: Observe and influence graph execution
- **Fallback to Local**: Continue with local reasoning if external unavailable

#### Benefits of External Orchestration
- **Advanced Features**: Checkpointing, time-travel debugging
- **Visual Design**: Graphical workflow design tools
- **Production Reliability**: Built-in retry, circuit breaking, scaling
- **Multi-tenancy**: Isolation between different workflow instances
- **Observability**: Detailed execution tracing and metrics

## Integration Patterns

### Harness + Loop Combination
- **Verification-Driven Development**: Harness provides verification, loops use it for improvement
- **Context-Managed Learning**: Harness manages context, loops refine understanding
- **Tool-Enhanced Iteration**: Harness provides tools, loops apply them repeatedly
- **State-Powered Progress**: Harness persists state, loops build upon it

### Loop + Graph Combination
- **Nested Loops**: Inner loops (micro-corrections) within outer graph steps
- **Graph-Controlled Loops**: Graph structure determines when loops activate
- **Loop Results as Graph Edges**: Loop outcomes influence graph transitions
- **Parallel Loop Execution**: Multiple agents running loops in graph branches

### Harness + Graph Combination
- **Graph-Defined Tool Availability**: Graph state controls which tools are accessible
- **Harness-Managed Graph Resources**: Harness provides compute/memory for graph execution
- **State Synchronization**: Harness state mechanisms keep graph state consistent
- **Observability Integration**: Harness observability monitors graph execution

## Claude Code-Specific Implementation Guides

### Setting Up a Verification Loop
1. Create verification skill with test/fix workflow
2. Add post-tool hook to trigger verification after edits
3. Configure max iterations and failure escalation
4. Implement layered verification (lint → test → build → e2e)
5. Add maker-checker separation if possible

### Creating a Research Loop
1. Build research skill with exploration/deep-dive/synthesis/validation phases
2. Add web search and source evaluation capabilities
3. Implement structured note-taking with citations
4. Add cross-verification and contradiction resolution
5. Include expert review or knowledgeable agent validation

### Designing a Subagent Delegation Graph
1. Identify required specialized roles (planner, architect, coder, tester, etc.)
2. Create agent definitions for each role
3. Define clear handoff protocols and task description formats
4. Implement result aggregation and integration strategies
5. Add quality checkpoints and review stages

### Building a Skill Routing System
1. Analyze incoming tasks to determine required capabilities
2. Match task characteristics to skill descriptions and frontmatter
3. Implement skill chaining for complex workflows
4. Add performance monitoring and optimization
5. Include fallback mechanisms for skill failures

## Anti-Patterns and Failure Modes

### Harness Engineering Anti-Patterns
- **Over-documentation**: Excessive CLAUDE.md reducing effectiveness
- **Tool Bloat**: Too many tools causing confusion and security risks
- **Environment Drift**: Setup documentation not matching actual environment
- **State Loss**: Critical information not persisted across sessions
- **Feedback Avoidance**: No reliable way to know if work is correct

### Loop Engineering Anti-Patterns
- **Infinite Loops**: Missing termination conditions or ineffective verification
- **Metric Gaming**: Optimizing for verification measures that don't reflect true quality
- **Context Amnesia**: Losing track of original goal across iterations
- **Over-refinement**: Perfecting insignificant details while missing big picture
- **Human Bottleneck**: Waiting for human input creating delays

### Graph Engineering Anti-Patterns
- **Over-engineering**: Using complex graphs for simple linear tasks
- **State Inconsistency**: Conflicting updates to shared graph state
- **Deadlocks**: Circular dependencies preventing progress
- **Complexity Trap**: Spending more time managing graph than doing work
- **Observability Blindness**: Inability to understand graph execution state

## Testing and Validation

### Harness Testing
- **Instruction Clarity**: Do LLMs correctly interpret and follow CLAUDE.md?
- **Tool Availability**: Are expected tools accessible and functional?
- **Environment Reproducibility**: Can new sessions reach executable state reliably?
- **State Persistence**: Does critical information survive session boundaries?
- **Feedback Reliability**: Does verification correctly identify success/failure?

### Loop Testing
- **Termination Guarantee**: Do loops always terminate under expected conditions?
- **Progress Assurance**: Do loops make measurable progress toward goals?
- **Quality Improvement**: Do outputs genuinely improve across iterations?
- **Failure Recovery**: Do loops handle expected failure modes gracefully?
- **Resource Bounds**: Do loops stay within reasonable time/token/memory limits?

### Graph Testing
- **State Consistency**: Is graph state correctly maintained across executions?
- **Routing Accuracy**: Does the graph follow intended paths based on inputs?
- **Deadlock Detection**: Are circular dependencies properly identified and handled?
- **Result Correctness**: Do graph executions produce correct outputs?
- **Performance Characteristics**: What are the time/space complexity properties?

## Claude Code Configuration Tips

### Performance Optimization
- **Context Window Management**: Monitor usage, summarize when approaching limits
- **Tool Call Optimization**: Batch operations, cache expensive results
- **Skill Loading**: Lazy load heavy skills only when needed
- **MCP Connection Pooling**: Reuse connections to external services
- **Hook Efficiency**: Keep hook scripts fast and focused

### Reliability Enhancements
- **Graceful Degradation**: Continue with reduced functionality when components fail
- **Fallback Mechanisms**: Alternative approaches when primary methods fail
- **Health Checks**: Regular validation of harness components
- **Circuit Breaking**: Temporarily disable failing components
- **Retry Logic**: Exponential backoff with jitter for transient failures

### Observability Improvements
- **Execution Logging**: Detailed traces of agent decisions and actions
- **Metric Collection**: Timing, success rates, resource usage
- **State Monitoring**: Watch for corruption or unexpected values
- **Error Tracking**: Categorize and track failures by type and location
- **User Feedback**: Collect satisfaction and usefulness metrics

## References

1. Claude Code Documentation: https://code.claude.com/docs/
2. Harness Engineering Skill: C:\Users\Lenovo\.claude\skills\harness-engineering\SKILL.md
3. Loop Skill: C:\Users\Lenovo\.claude\skills\Loop\SKILL.md
4. Martin Fowler: [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)
5. Addy Osmani: [Agent Harness Engineering](https://addyosmani.com/blog/agent-harness-engineering/)
6. LangChain: [The Anatomy of an Agent Harness](https://www.langchain.com/blog/the-anatomy-of-an-agent-harness)
7. Teklens.ai: [Graph Engineering: Multi-Agent Workflows, Not Chat Chains](https://teklens.ai/en/pm-lab/graph-engineering)
8. ArXiv: [Graph Engineering in the Era of LLM Agents](https://arxiv.org/pdf/2608.21156v2)

---