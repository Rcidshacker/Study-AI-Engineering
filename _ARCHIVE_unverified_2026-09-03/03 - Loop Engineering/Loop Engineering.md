---
title: Loop Engineering
aliases:
  - Agent Loop
  - Feedback Loop
tags:
  - ai-engineering
  - agent-engineering
  - loop-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Loop Engineering

## Definition

Loop engineering is the practice of designing and optimizing the iterative cycles (loops) that AI agents execute to achieve goals. An agent loop typically follows a pattern of: **Planning → Execution → Observation → Verification → Correction** (or similar variations). The focus is on building systems that enable agents to refine their work through feedback rather than relying on single-shot prompts.

## Origin and Popularization

The term "loop engineering" gained prominence in mid-2026, particularly around June 2026. Key contributors include:

- **Peter Steinberger** (OpenClaw agent project) argued in June 2026 that the key skill had shifted from prompting agents to designing the loops that prompt them.
- **Addy Osmani** (Google engineer) published an essay titled "Loop Engineering" that provided an anatomy: automations, worktrees, skills, connectors, sub-agents, and external state.
- **Boris Cherny** (Anthropic, Claude Code team) reportedly summarized the shift as: "I don't prompt Claude anymore."
- Various engineering blogs and guides from Lushbinary, AppScale, Tosea.ai, and BuildFastWithAI.

## Core Components of an Agent Loop

A well-designed agent loop typically includes:

1. **Goal Definition**
   - A specific, testable objective that defines success.
   - Vague objectives lead to vague loops; success criteria must be clear.

2. **Tool Selection**
   - Choosing the right tools the agent needs to accomplish the goal (file access, search APIs, code execution, etc.).

3. **Context Management**
   - Deciding what information stays in the working context vs. what gets summarized or dropped after each turn.
   - Includes context window management, summarization strategies, and external memory.

4. **Evaluation/Verification Step**
   - Mechanisms to judge whether an action succeeded: tests, linters, type checkers, rules, or second-model validation.
   - This is the critical feedback mechanism that enables improvement.

5. **Branching Logic**
   - Specifying what happens after:
     - Success (continue, terminate, or move to next step)
     - Recoverable failure (retry, adjust approach, try alternative)
     - Hard blocker (escalate to human, abort, or seek assistance)

6. **Termination Conditions**
   - Conditions that end the loop:
     - Completed task (goal achieved)
     - Maximum number of iterations (prevents infinite loops)
     - Confidence threshold (sufficiently good solution)
     - Resource limits (time, cost, token budget)

## Types of Loops

### By Scope
- **Inner Loops**: Tight, fast cycles focused on immediate corrections (e.g., fixing a syntax error).
- **Outer Loops**: Broader cycles that encompass larger workflows (e.g., implementing a feature through multiple inner loops).

### By Function
- **Planning Loops**: Iteratively refining a plan before execution.
- **Execution Loops**: Carrying out planned actions and observing results.
- **Verification Loops**: Repeatedly checking correctness against criteria.
- **Research Loops**: Iteratively gathering, cross-checking, and synthesizing information.
- **Self-Review Loops**: Agents reviewing and improving their own outputs.
- **Test/Fix Loops**: Writing code, running tests, fixing failures, and repeating.
- **Human-in-the-Loop (HITL)**: Incorporating human judgment at key points.
- **Autonomous Loops**: Fully automated cycles requiring no human intervention.

### By Architecture
- **Sequential Loops**: Simple linear iteration.
- **Nested Loops**: Loops within loops (e.g., outer feature development loop with inner test/fix loops).
- **Conditional Loops**: Branching based on intermediate results.
- **Parallel Loops**: Multiple agents working on different aspects simultaneously.

## Importance Over Individual Prompts

Loop engineering is more important than individual prompt craftsmanship because:

1. **Error Correction**: Real-world tasks involve mistakes; loops enable agents to detect and correct them.
2. **Adaptability**: Loops allow agents to adjust to unexpected situations or changing requirements.
3. **Quality Assurance**: Verification steps ensure outputs meet standards rather than relying on hope.
4. **Complex Task Handling**: Break complex goals into manageable iterations.
5. **Reduced Supervision**: Well-designed loops enable longer autonomous operation.
6. **Predictability**: Loops with clear termination conditions are more reliable than open-ended prompting.

## Claude Code Implementation

Claude Code enables loop engineering through several mechanisms:

1. **Hooks System**
   - Pre-tool hooks: Run validation before tool execution (e.g., check if file exists).
   - Post-tool hooks: Run verification after tool execution (e.g., run tests after editing).
   - Example: A hook that automatically runs `pytest` after any file edit.

2. **CLAUDE.md Configuration**
   - Can define verification criteria and loop behaviors.
   - Example: Instructing Claude to "keep running tests until they pass" establishes a test/fix loop.

3. **Skills for Verification**
   - Custom skills that encapsulate verification logic (e.g., a skill that runs linters and returns results).
   - Skills can be chained together in loops.

4. **Agent Delegation**
   - Subagents can be tasked with specific verification or correction loops.
   - Example: A main agent implements a feature, delegating testing to a subagent.

5. **MCP Servers for External Verification**
   - Connecting to external systems for verification (e.g., CI/CD pipelines, code review systems).

6. **Built-in Loop Patterns**
   - Claude Code inherently supports loops through its agentic nature: it will continue working on a task until satisfied or stopped.

## Practical Examples

### Test/Fix Loop in Claude Code
```markdown
# .claude/hooks/post_edit.py
#!/usr/bin/env python3
import subprocess
import sys

def run_tests():
    result = subprocess.run(['pytest'], capture_output=True, text=True)
    if result.returncode != 0:
        print("Tests failed:", result.stdout)
        print("Errors:", result.stderr)
        return False
    return True

if __name__ == "__main__":
    if not run_tests():
        sys.exit(1)  # Signal failure to trigger loop continuation
```

### Research Loop
```markdown
# .claude/skills/research_loop.md
When researching a topic:
1. Search for initial sources
2. Read and summarize key points
3. Identify gaps or contradictions
4. Search for additional sources to address gaps
5. Cross-check facts across multiple sources
6. Repeat until comprehensive understanding is achieved
7. Synthesize findings into final output
```

## Failure Modes

- **Infinite Loops**: Missing or ineffective termination conditions.
- **Wrong Success Criteria**: Verification that doesn't align with true goals.
- **Over-Optimization**: Loops that perfect insignificant details while missing the big picture.
- **Context Drift**: Losing track of the original goal across iterations.
- **Tool Failure Loops**: Getting stuck retrying a broken tool approach.
- **Verification Bottlenecks**: Slow verification steps making loops prohibitively expensive.

## When to Use Which Loop Architecture

- **Simple Tasks**: Linear test/fix loops may suffice.
- **Complex Features**: Nested loops (outer development loop, inner verification loops).
- **Research Tasks**: Exploratory loops with branching based on findings.
- **Collaborative Work**: Human-in-the-loop at key decision points.
- **Performance-Critical**: Minimize loop iterations through better planning.
- **Learning Tasks**: Loops that emphasize reflection and self-improvement.

## Relationship to Harness and Graph Engineering

- Loops are a **critical component** within the agent harness.
- Graph engineering can be seen as orchestrating multiple loops or replacing linear loops with graph-based workflows when parallelism or complex routing is needed.
- A single agent loop is the simplest possible graph (one node with a self-edge).

## References

1. Peter Steinberger: OpenClaw agent project insights (June 2026)
2. Addy Osmani: [Loop Engineering](https://addyosmani.com/blog/loop-engineering/)
3. Lushbinary: [Loop Engineering: The Guide for AI Agents](https://lushbinary.com/blog/loop-engineering-ai-coding-agents-guide/)
4. AppScale: [Loop Engineering for AI Agents: The Complete Guide](https://appscale.blog/en/blog/loop-engineering-ai-agents-complete-guide-2026)
5. Tosea.ai: [What Is Loop Engineering? A Complete Guide](https://tosea.ai/blog/loop-engineering-ai-agents-complete-guide-2026)
6. BuildFastWithAI: [Loop Engineering: Complete Guide for AI Agents (2026)](https://www.buildfastwithai.com/blogs/loop-engineering-ai-agents-guide)
7. Analytics Insight: [Loop Engineering: Building Smarter AI Agent Workflows](https://www.analyticsinsight.net/artificial-intelligence/loop-engineering-the-complete-guide-to-building-smarter-ai-agent-workflows)

---