---
title: Claude Code MOC
aliases:
  - Claude Code Ecosystem
  - Agentic Coding Platform
tags:
  - moc
  - claude-code
  - ai-engineering
  - agent-engineering
  - harness-engineering
  - loop-engineering
  - graph-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Claude Code MOC

This map focuses on Claude Code specifically - how to use, extend, and optimize it for agentic software development workflows.

## Core Architecture

### [[Claude Code Architecture]]
The fundamental design of Claude Code as an agentic coding assistant:
- Model + Harness composition
- Built-in tools and capabilities
- Extension points and customization mechanisms
- Execution loop and context management

## Extension Mechanisms

### [[Claude Code Skills]]
Reusable knowledge and invocable workflows:
- Skill structure and frontmatter
- Workflows, tools, and references
- Creation and management best practices
- [[CreateSkill]] skill for proper skill development

### [[Claude Code Hooks]]
Automation at lifecycle events:
- Hook types (command, HTTP, MCP, prompt)
- Event matching and configuration
- Common hook patterns (linting, testing, security)
- Hook debugging and optimization

### [[Claude Code Agents]]
Specialized subagents for isolated work:
- Built-in agents (planner, architect, tester, etc.)
- Custom agent creation and delegation
- Agent communication and result handling
- Context isolation and persistence

### [[Claude Code MCP]]
Model Context Protocol for external integrations:
- MCP server architecture and capabilities
- Common MCP servers (filesystem, git, github, databases)
- Creating custom MCP servers
- MCP security and permission model

### [[Claude Code Commands]]
Custom slash operations:
- Command skills vs shell scripts
- Parameter handling and validation
- Integration with skills and hooks
- Command discovery and organization

## Engineering Concepts in Claude Code

### [[Claude Code Harnesses]]
How harness engineering manifests in Claude Code:
- CLAUDE.md as instruction layer
- Skills as tool and workflow components
- Hooks for automation and verification
- MCP for extended tool capabilities
- Subagents for orchestration

### [[Claude Code Loops]]
Loop engineering implementations:
- Built-in agentic loop (plan-act-observe-verify)
- Verification loops via hooks and skills
- Research loops for information gathering
- Test/fix loops for quality assurance
- The `/loop` skill for explicit iteration

### [[Claude Code Graphs]]
Graph engineering possibilities:
- Subagent delegation graphs
- Skill routing and selection graphs
- Workflow graphs encoded in skills
- External graph orchestration via MCP
- State persistence for graph workflows

## Practical Implementation

### [[Practical Examples MOC]]
Working Claude Code examples:
- [[Coding Agent Harness]] - Complete web app harness
- [[Autonomous Test Fixer]] - Self-correcting test loop
- [[Research Agent]] - Deep research workflow
- [[Skill Router]] - Context-based skill selection
- [[Multi Agent Coding System]] - Coordinated agent collaboration

### [[Scenarios MOC]]
Situation-specific guidance:
- [[Production Coding Agent]] - Reliable workflows for production
- [[Vibe Coding Workflow]] - Simplified setup for exploration
- [[Autonomous Development Workflow]] - Hands-off agent operation
- [[Multi Agent Development]] - Team-based agent collaboration

## Configuration and Optimization

### [[Claude Code Configuration]]
Managing settings and preferences:
- Settings hierarchy and precedence
- Performance optimization techniques
- Context window management
- Token usage optimization
- Debugging and troubleshooting

### [[Claude Code Best Practices]]
Proven patterns for effective usage:
- Project organization and structure
- Skill and hook design principles
- Verification and quality assurance
- State persistence strategies
- Team collaboration patterns

## Ecosystem and Integrations

### [[Claude Code Plugins]]
Packaging and distributing extensions:
- Plugin structure and manifest
- Marketplace publication and installation
- Version compatibility and updates
- Popular and recommended plugins

### [[Claude Code and Other Tools]]
Integration with development ecosystems:
- Git and GitHub workflows
- CI/CD pipeline integration
- IDE and editor compatibility
- Testing framework compatibility
- Deployment and release processes

## Learning and Mastery

### [[Claude Code Learning Path]]
Progression from beginner to expert:
- [[Claude Code for Beginners]]
- [[Intermediate Claude Code Usage]]
- [[Advanced Claude Code Techniques]]
- [[Claude Code Power User]]

### [[Claude Code Tips and Tricks]]
Useful insights and shortcuts:
- Productivity enhancements
- Common pitfalls to avoid
- Undocumented features and behaviors
- Community-discovered patterns

## Related Maps

- [[AI Engineering MOC]]
- [[Agent Engineering MOC]]
- [[Harness Loop Graph MOC]]

---