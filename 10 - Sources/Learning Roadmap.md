---
title: Learning Roadmap
aliases:
  - What should I learn next?
  - Progression Guide
tags:
  - moc
  - learning
  - roadmap
  - ai-engineering
  - agent-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Learning Roadmap

This document provides a structured progression from beginner to advanced to production usage of harness engineering, loop engineering, and graph engineering concepts in agentic software development, with specific application to Claude Code.

## Phase 1: Beginner Foundation (Weeks 1-2)

### Goals
- Understand core concepts
- Set up basic Claude Code harness
- Implement simple loops
- Run first agentic workflows

### Topics to Study
1. **AI Engineering Basics**
   - Read: [[AI Engineering MOC]]
   - Understand: What is an AI agent? Model + Harness = Agent

2. **Harness Engineering Fundamentals**
   - Read: [[Harness Engineering]] (focus on definition and components)
   - Practice: Create a basic CLAUDE.md for a small project
   - Experiment: Add one useful skill (e.g., a simple verification skill)

3. **Loop Engineering Basics**
   - Read: [[Loop Engineering]] (focus on agent loop definition)
   - Practice: Implement a simple test/fix loop using hooks
   - Experiment: Use the `/loop` skill on a small writing task

4. **Claude Code Essentials**
   - Read: [[Claude Code Architecture]]
   - Practice: Set up hooks for linting on file save
   - Experiment: Create a simple skill for a repetitive task

### Deliverables
- A small project with basic CLAUDE.md
- One custom skill (e.g., "greeting-skill" that outputs a greeting)
- One hook (e.g., lint-on-save)
- Documentation of what you learned

## Phase 2: Intermediate Application (Weeks 3-6)

### Goals
- Apply all three engineering paradigms
- Build reliable agent workflows
- Solve real problems with agent assistance
- Begin integrating multiple concepts

### Topics to Study
1. **Applied Harness Engineering**
   - Read: [[Harness Components]] and [[Harness Patterns]]
   - Practice: Implement proper state persistence (claude-progress.md, DECISIONS.md)
   - Experiment: Set up MCP servers for external services you use

2. **Applied Loop Engineering**
   - Read: [[Agent Loop Patterns]] and [[Verification Loops]]
   - Practice: Create a multi-layer verification loop (lint → test → build)
   - Experiment: Build a research loop for a topic you're interested in

3. **Applied Graph Engineering**
   - Read: [[Agent Graphs]] and [[Skill Graphs]]
   - Practice: Design a simple skill routing graph
   - Experiment: Create a subagent delegation graph for a small feature

4. **Claude Code Integration**
   - Read: [[Claude Code Implementation Notes]]
   - Practice: Combine skills, hooks, and subagents in a workflow
   - Experiment: Use the verification-loop skill from the harness-engineering skill

### Deliverables
- A medium-sized project with:
  - Comprehensive CLAUDE.md
  - 3-5 custom skills covering different aspects
  - Hooks for automation and verification
  - State persistence files
  - Evidence of loop and graph engineering application
- A written reflection on what worked and what didn't

## Phase 3: Advanced Implementation (Weeks 7-12)

### Goals
- Build production-grade agent systems
- Optimize for reliability and performance
- Contribute to the ecosystem
- Handle complex, real-world scenarios

### Topics to Study
1. **Advanced Harness Engineering**
   - Read: [[Harness Failure Modes]] and [[Professional Software Engineers]] section
   - Practice: Implement security boundaries and permission systems
   - Experiment: Create observability and monitoring for your agent system

2. **Advanced Loop Engineering**
   - Read: [[Loop Failure Modes]] and [[AI Engineers]] section
   - Practice: Implement adaptive loops that learn from failures
   - Experiment: Build human-in-the-loop systems with clear escalation paths

3. **Advanced Graph Engineering**
   - Read: [[Graph Orchestration]] and [[AI Engineers]] section
   - Practice: Build dynamic graphs that adapt based on execution
   - Experiment: Create knowledge graphs for domain-specific reasoning

4. **Professional Claude Code Usage**
   - Read: [[Production Coding Agent]] scenario
   - Practice: Implement agent role specialization (planner, architect, tester, etc.)
   - Experiment: Set up CI/CD integration with agent workflows

### Deliverables
- A complex project demonstrating:
  - Multi-agent orchestration with clear roles
  - Comprehensive verification and feedback systems
  - State persistence and recovery mechanisms
  - Integration with external tools and services
- A blog post or documentation explaining your system
- A contribution to an open-source agent engineering project (skill, hook, documentation)

## Phase 4: Production Mastery (Ongoing)

### Goals
- Maintain and evolve production agent systems
- Mentor others in agent engineering
- Push the boundaries of what's possible
- Stay current with emerging practices

### Activities
1. **System Maintenance**
   - Regularly review and simplify your harness (remove what's not needed)
   - Update skills and hooks based on new learnings
   - Monitor system performance and reliability

2. **Community Engagement**
   - Share your learnings through blogs, talks, or workshops
   - Answer questions in agent engineering communities
   - Contribute to shared resources (skills, hooks, patterns)

3. **Experimentation and Research**
   - Try new approaches to harness, loop, and graph engineering
   - Experiment with emerging MCP servers and tools
   - Test cutting-edge agent frameworks and compare to Claude Code

4. **Teaching and Mentoring**
   - Help others set up their first agent harness
   - Guide intermediate learners through common pitfalls
   - Advise on architectural decisions for agent systems

### Ongoing Deliverables
- A continuously improving agent system for your projects
- Shared resources that others can use and build upon
- A growing knowledge base of what works and what doesn't
- Contributions to the agent engineering community

## Resource Progression

### Beginner Resources
- Official Claude Code documentation
- [[Harness Engineering]], [[Loop Engineering]], [[Graph Engineering]] core notes
- [[Claude Code Architecture]]
- Practical examples from [[Practical Examples MOC]]

### Intermediate Resources
- Repository analyses from [[GitHub Repository Index]]
- [[Claude Code Implementation Notes]]
- Scenario applications from [[Scenarios MOC]]
- Comparison notes from [[Comparisons MOC]]

### Advanced Resources
- Original sources linked in the [[Sources MOC]]
- Engineering blogs from companies using agents at scale
- Academic papers on agent systems and verification
- Framework documentation (LangGraph, AutoGen, ADK) for comparison

## Assessment Checkpoints

### After Phase 1 (Beginner)
- [ ] Can explain the Model + Harness = Agent formula
- [ ] Has created a functional CLAUDE.md
- [ ] Has built at least one custom skill
- [ ] Has used hooks for automation
- [ ] Has experimented with the `/loop` skill

### After Phase 2 (Intermediate)
- [ ] Has implemented state persistence mechanisms
- [ ] Has built multi-layer verification loops
- [ ] Has designed and used skill routing or agent delegation graphs
- [ ] Has combined multiple engineering concepts in a workflow
- [ ] Has solved a real problem using agent assistance

### After Phase 3 (Advanced)
- [ ] Has built a production-grade agent system with multiple safeguards
- [ ] Has implemented advanced patterns like adaptive loops or dynamic graphs
- [ ] Has contributed back to the community
- [ ] Can troubleshoot complex agent system issues
- [ ] Has taught others agent engineering concepts

### After Phase 4 (Production)
- [ ] Maintains reliable agent systems in real projects
- [ ] Continuously improves and simplifies harness over time
- [ ] Actively shares knowledge and contributes to community
- [ ] Stays current with emerging practices and technologies

## Final Thoughts

Remember that agent engineering is as much about mindset as it is about technique. The most important skills are:
1. **Systems thinking**: Understanding how components interact
2. **Experimentation mindset**: Trying things, learning from results, iterating
3. **Simplicity seeking**: Always asking "What's the simplest thing that could work?"
4. **Feedback orientation**: Building systems that learn from their own performance
5. **Human-centered design**: Remembering that agents serve human goals

Start small, focus on solving real problems, and let your understanding grow through practice. The journey from simple prompts to sophisticated agent systems is incremental—each small improvement builds toward greater capability.

Happy engineering!