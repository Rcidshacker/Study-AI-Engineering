---
title: Claude Code Architecture
aliases:
  - Claude Code Internals
  - Agentic Loop
tags:
  - claude-code
  - ai-engineering
  - agent-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Claude Code Architecture

## Overview

Claude Code is Anthropic's agentic coding assistant that combines a powerful language model with a sophisticated harness to enable autonomous software development workflows. Understanding its architecture is crucial for effectively extending and customizing it for specific use cases.

## Core Components

### 1. The Model
- **Base**: Claude 3 series (Sonnet, Opus) or Claude 4 series
- **Capabilities**: Reasoning, code generation, tool use, following complex instructions
- **Limitations**: Stateless, finite context window, potential for hallucination

### 2. The Harness (Everything Outside the Model)
Claude Code's harness includes:

#### Built-in Tools
- **File Operations**: Read, write, edit, search files
- **Web Search**: Access current information (when enabled)
- **Code Execution**: Run commands, execute scripts
- **Browser Automation**: Interact with web pages (via Interceptor skill)
- **Git Integration**: Version control operations

#### Agentic Loop
The core execution cycle:
1. **Understand**: Parse user request and current context
2. **Plan**: Determine what needs to be done
3. **Execute**: Use tools to perform actions
4. **Observe**: See results of actions
5. **Verify**: Check if goal is met (via built-in verification or hooks)
6. **Iterate**: Continue or adjust based on verification

### 3. Extension Points

#### CLAUDE.md
- **Purpose**: Persistent project-specific context
- **Location**: Project root or parent directories
- **Loading**: Additive - all CLAUDE.md files from working directory upward are loaded
- **Best Practices**: 
  - Keep concise (50-200 lines)
  - Put critical constraints at top/bottom (avoid "lost in the middle")
  - Use as a router to topic-specific docs
  - Reference rather than duplicate information

#### Skills
- **Purpose**: Reusable knowledge and invocable workflows
- **Location**: 
  - Global: `~/.claude/skills/`
  - Project: `.claude/skills/`
  - Plugins: Inside plugin directories
- **Structure**: 
  - `SKILL.md` (frontmatter + workflows)
  - `Workflows/` (specific procedures)
  - `Tools/` (reusable functions)
  - `References/` (documentation)
  - `assets/` (template files)
- **Invocation**: 
  - Automatic (when relevant based on description)
  - Manual (`/skill-name` command)
  - Agent-initiated (subagents can invoke skills)

#### Hooks
- **Purpose**: Automate actions at lifecycle events
- **Events**: 
  - Pre-tool, Post-tool
  - Pre-agent, Post-agent
  - Session start/end
  - Custom events
- **Types**:
  - Command hooks (shell scripts)
  - HTTP hooks (web requests)
  - MCP tool hooks (external service calls)
  - Prompt hooks (modify agent prompts)
- **Location**: `~/.claude/hooks/` or `.claude/hooks/`

#### Subagents
- **Purpose**: Isolated context for specialized tasks
- **Types**:
  - Built-in: planner, architect, code-reviewer, etc.
  - Custom: User-defined specialized agents
- **Communication**: 
  - Main agent delegates task via `/agent` command or Skill invocation
  - Subagent runs isolated loop
  - Returns summary to main agent
- **Isolation**: Separate context window prevents contamination

#### MCP (Model Context Protocol)
- **Purpose**: Standardized connection to external services and tools
- **Servers**: 
  - Local: `~/.claude/mcp/` or `.claude/mcp/`
  - Remote: HTTP-based MCP servers
- **Capabilities**: 
  - Exposes external tools as if they were built-in
  - Standardized tool description format
  - Bidirectional communication
- **Common Servers**: 
  - Filesystem, Git, GitHub, Docker, Kubernetes, databases

#### Plugins
- **Purpose**: Package and distribute complete Claude Code setups
- **Components**: 
  - Skills, hooks, MCP configs, agents, commands
  - Metadata (`plugin.json`, `marketplace.json`)
- **Installation**: 
  - `claude plugin add <name>`
  - Manual copy to `~/.claude/plugins/`

#### Commands
- **Purpose**: Custom slash commands for frequent operations
- **Location**: `~/.claude/commands/` or `.claude/commands/`
- **Types**:
  - Command skills (disable-model-invocation: true)
  - Shell scripts
  - Complex workflows

## Configuration Hierarchy

Settings are resolved in this order (later overrides earlier):
1. Built-in defaults
2. Global user settings (`~/.claude/settings.json`)
3. Project settings (`.claude/settings.json`)
4. CLI flags
5. Session-specific overrides

## Context Management

### Context Sources
1. **System Prompt**: Core behavior and capabilities
2. **CLAUDE.md Files**: Project conventions and instructions
3. **Skills**: Loaded on demand when relevant
4. **Conversation History**: Current session messages
5. **Tool Results**: Outputs from previous actions
6. **MCP Server State**: External service data
7. **Subagent Results**: Summaries from delegated work

### Context Limitations
- **Window Size**: Limited by model's context capacity
- **Compaction**: Automatic summarization when window fills
- **Strategies**: 
  - Recent messages preserved in full
  - Older content summarized or dropped
  - Important facts can be preserved in CLAUDE.md or skills
  - Cross-session state via files (`claude-progress.md`, `DECISIONS.md`)

## Execution Model

### Single Agent Mode
- Default behavior: one main agent handling the task
- Can invoke subagents for isolated work
- Maintains single conversation thread

### Multi-Agent Patterns
- **Delegation**: Main agent → Subagent → Result
- **Parallelization**: Multiple subagents working simultaneously
- **Pipelines**: Output of one agent feeds into another
- **Feedback Loops**: Agent A checks work of Agent B

## Verification and Feedback Mechanisms

### Built-in Verification
- Basic syntax checking for supported languages
- Simple test detection (look for test files/commands)

### Extended Verification via Hooks
- Pre-tool: Validate inputs, check preconditions
- Post-tool: Run tests, linting, security scans
- Custom: Any verification logic implementable in scripts

### Evaluator Pattern
- Separate agent/judgment for checking work
- Critical for avoiding self-assessment bias
- Can be different model or same model with different prompt

## State Persistence

### Within Session
- Conversation history
- Tool execution results
- Agent internal state

### Across Sessions
- **Files**: `claude-progress.md`, `DECISIONS.md`, feature lists
- **Git**: Commits as checkpoints
- **External**: Databases, vector stores via MCP
- **Skills**: Knowledge that persists in skill definitions

## Relationship to Engineering Concepts

### Harness Engineering
Claude Code itself is a harness. Users extend it via:
- CLAUDE.md (instructions)
- Skills (tools/workflows)
- Hooks (automation/verification)
- MCP (external connections)
- Subagents (orchestration)

### Loop Engineering
Inherent in Claude Code's design:
- Agentic loop: plan-act-observe-verify
- Custom loops via:
  - Hooks creating feedback cycles
  - Skills defining iterative procedures
  - Subagents with looping behavior
  - `/loop` skill for explicit iteration

### Graph Engineering
Can be implemented through:
- **Subagent delegation graphs**: Main agent → [specialized subagents] → Result
- **Skill routing graphs**: Agent decides which skill to invoke based on context
- **Workflow graphs**: Complex procedures encoded in skills
- **External orchestration**: Using MCP to connect to LangGraph/AutoGen

## Customization Best Practices

### Start Small
1. Begin with CLAUDE.md for project conventions
2. Add skills for repetitive workflows
3. Use hooks for automation (linting, testing)
4. Explore MCP for external integrations
5. Consider subagents for complex delegations

### Maintainability
- Version control your `.claude/` directory
- Document skill dependencies
- Keep skills focused and single-purpose
- Test extensions in isolation
- Use clear naming conventions

### Performance
- Be mindful of context usage
- Lazy load heavy skills only when needed
- Cache expensive operations
- Consider token costs of frequent tool use

## Troubleshooting

### Common Issues
- **Context overflow**: Too much information in CLAUDE.md/skills
- **Tool permission errors**: Missing MCP server or incorrect config
- **Skill not invoking**: Incorrect description or frontmatter
- **Hooks not firing**: Wrong matcher or event type
- **Subagent isolation problems**: Unexpected context sharing

### Debugging Approaches
1. Check Claude Code logs (`~/.claude/daemon.log`)
2. Use verbose mode for detailed execution traces
3. Test components in isolation
4. Verify file paths and permissions
5. Start with minimal configuration and add incrementally

## References

1. Claude Code Documentation: https://code.claude.com/docs/
2. Steering Claude Code: https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more
3. Hooks Reference: https://code.claude.com/docs/en/hooks
4. Skills System: https://code.claude.com/docs/en/skills
5. MCP Documentation: https://code.claude.com/docs/en/mcp
6. Subagents Guide: https://code.claude.com/docs/en/sub-agents

---