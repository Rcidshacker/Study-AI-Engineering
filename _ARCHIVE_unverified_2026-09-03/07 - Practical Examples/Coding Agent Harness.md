---
title: Coding Agent Harness Example
aliases:
  - Example Harness
  - Claude Code Setup
tags:
  - claude-code
  - harness-engineering
  - practical-example
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Coding Agent Harness Example

This example demonstrates a complete harness setup for a web application project using Claude Code, incorporating harness, loop, and graph engineering principles.

## Project Structure

```text
my-web-app/
├── .claude/
│   ├── CLAUDE.md
│   ├── agents/
│   │   ├── planner.md
│   │   ├── architect.md
│   │   ├── frontend-dev.md
│   │   ├── backend-dev.md
│   │   ├── tester.md
│   │   └── reviewer.md
│   ├── skills/
│   │   ├── verification-loop/
│   │   │   ├── SKILL.md
│   │   │   └── Workflows/
│   │   │       └── test-fix-loop.md
│   │   ├── research/
│   │   │   ├── SKILL.md
│   │   │   └── Workflows/
│   │   │       └── deep-research.md
│   │   └── feature-development/
│   │       ├── SKILL.md
│   │       └── Workflows/
│   │           └── full-stack-feature.md
│   ├── hooks/
│   │   ├── lint-on-edit.json
│   │   ├── test-on-save.json
│   │   └── security-check.json
│   ├── commands/
│   │   ├── /research
│   │   ├── /build-feature
│   │   └── /run-tests
│   └── mcp/
│       ├── github.json
│       └── postgres.json
├── src/
│   ├── components/
│   ├── pages/
│   ├── services/
│   └── utils/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── CLAUDE.md
├── AGENTS.md
├── DECISIONS.md
├── claude-progress.md
├── feature-list.json
├── package.json
└── README.md
```

## 1. Project-Level CLAUDE.md (`my-web-app/CLAUDE.md`)

```markdown
# My Web Application

**Tech Stack**: React 18, Node.js 20, PostgreSQL, Tailwind CSS
**Architecture**: Feature-based folders, API-first design
**Conventions**: 
- All components must have corresponding test files
- API endpoints require input validation and error handling
- New features must pass end-to-end verification before merging
- Use functional components with hooks, avoid class components

## Verification Definition of Done
A feature is "done" only when:
1. Unit tests pass (>80% coverage)
2. Integration tests pass
3. End-to-end user flows complete successfully
4. Code review approves (no critical/security issues)
5. Documentation updated

## Active Development
Current focus: User authentication system
Next up: Payment processing integration

See `.claude/agents/` for specialized subagents.
See `.claude/skills/` for reusable workflows.
```

## 2. Global CLAUDE.md (`~/.claude/CLAUDE.md`)

```markdown
# Global Claude Code Configuration

## Identity
I am a senior full-stack engineer focused on building reliable, maintainable web applications.

## Core Principles
- Write tests before implementation (TDD)
- Keep functions small and focused (<20 lines)
- Prefer composition over inheritance
- Handle errors explicitly, never ignore them
- Security first: validate all inputs, escape outputs

## Tool Preferences
- Use built-in file tools for simple operations
- Delegate complex operations to appropriate skills
- Leverage MCP servers for external service interactions
- Use subagents for isolated, specialized work

## Verification Standards
- All code must pass ESLint + Prettier
- TypeScript code must pass tsc with no errors
- Jest/Vitest for unit and integration tests
- Playwright/Cypress for end-to-end tests

## Context Management
- Important project decisions live in DECISIONS.md
- Current progress tracked in claude-progress.md
- Feature priorities in feature-list.json
- Never rely on conversation history for critical information
```

## 3. Agents (`.claude/agents/`)

### planner.md
```markdown
# Planner Agent

**Role**: Break down high-level goals into actionable tasks
**When to invoke**: Starting new features, unclear requirements
**Output**: Detailed task list with dependencies and verification criteria

## Workflow
1. Understand the goal from context
2. Identify required components and interactions
3. Break into atomic tasks (≤2 hours each)
4. Identify dependencies and risks
5. Define verification methods for each task
6. Return structured task list
```

### architect.md
```markdown
# Architect Agent

**Role**: Make structural decisions, ensure consistency
**When to invoke**: New modules, architectural changes, scalability concerns
**Output**: Architecture diagrams, interface definitions, tech choices

## Responsibilities
- Define component interfaces and contracts
- Choose appropriate patterns and technologies
- Ensure scalability and maintainability
- Review for architectural drift
```

### frontend-dev.md
```markdown
# Frontend Developer Agent

**Role**: Implement user interfaces and client-side logic
**Skills**: React, TypeScript, Tailwind, state management
**When to invoke**: UI implementation, component creation
**Output**: Working frontend code with tests

## Practices
- Create reusable, composable components
- Implement proper loading and error states
- Follow accessibility guidelines (WCAG 2.1 AA)
- Write corresponding unit tests
```

### backend-dev.md
```markdown
# Backend Developer Agent

**Role**: Implement server-side logic and APIs
**Skills**: Node.js, Express, PostgreSQL, REST/GraphQL
**When to invoke**: API implementation, database work
**Output**: Working backend code with tests

## Practices
- Design RESTful/resource-oriented APIs
- Implement proper error handling and validation
- Use connection pooling and transactions
- Write comprehensive API tests
```

### tester.md
```markdown
# Tester Agent

**Role**: Design and execute comprehensive testing strategies
**When to invoke**: Test planning, complex test scenarios, test automation
**Output**: Test plans, test cases, automated test suites

## Focus Areas
- Unit test coverage and quality
- Integration test scenarios
- End-to-end user flows
- Performance and load testing
```

### reviewer.md
```markdown
# Code Reviewer Agent

**Role**: Evaluate code quality, security, and maintainability
**When to invoke**: Pre-merge review, quality gates, security audits
**Output**: Review comments, approval status, improvement suggestions

## Review Criteria
- Correctness and completeness
- Code clarity and maintainability
- Security vulnerabilities
- Performance implications
- Test adequacy
- Documentation quality
```

## 4. Skills (`.claude/skills/`)

### verification-loop/SKILL.md
```markdown
---
name: verification-loop
description: "Execute a test-fix loop until verification passes"
disable-model-invocation: true
---

# Test-Fix Verification Loop

This skill implements a robust verification loop that continues until all verification criteria are met.

## Workflow
1. Run verification suite (lint → test → build → e2e)
2. If all pass: return success
3. If failures: analyze, fix, and repeat
4. Max 5 iterations to prevent infinite loops
5. After max iterations: return failure with detailed report

## Sub-loops
- Linting loop: fix style issues
- Test loop: fix failing tests
- Build loop: fix compilation errors
- E2E loop: fix user flow issues
```

### verification-loop/Workflows/test-fix-loop.md
```markdown
## Test-Fix Loop Workflow

### Phase 1: Linting
1. Run ESLint + Prettier check
2. If failures:
   - Show specific violations
   - Fix each violation
   - Repeat until clean
3. Proceed to testing

### Phase 2: Unit Testing
1. Run vitest --coverage
2. If failures:
   - Show failing tests
   - Analyze root cause
   - Fix code or tests as appropriate
   - Repeat until all pass
3. Check coverage ≥80%
4. Proceed to integration testing

### Phase 3: Integration Testing
1. Run integration test suite
2. If failures:
   - Show failing scenarios
   - Fix integration issues
   - Repeat until pass
3. Proceed to E2E testing

### Phase 4: End-to-End Testing
1. Run E2E test suite (Playwright)
2. If failures:
   - Show failing user flows
   - Fix issues
   - Repeat until pass
3. Final verification complete
```

### research/SKILL.md
```markdown
---
name: research
description: "Conduct deep research on technical topics"
disable-model-invocation: true
---

# Deep Research Skill

Implements a research loop for gathering, synthesizing, and validating technical information.
```

### research/Workflows/deep-research.md
```markdown
## Deep Research Workflow

### Phase 1: Exploration
1. Define research questions and success criteria
2. Initial broad search for sources
3. Identify authoritative sources and experts
4. Scan for contradictions and gaps

### Phase 2: Deep Dive
1. Read and summarize key sources
2. Take structured notes with citations
3. Identify open questions and controversies
4. Search for additional sources to address gaps

### Phase 3: Synthesis
1. Cross-check facts across multiple sources
2. Resolve contradictions through evidence weighting
3. Identify consensus and disagreement areas
4. Create coherent narrative with supporting evidence

### Phase 4: Validation
1. Have expert or knowledgeable agent review findings
2. Check for completeness against success criteria
3. Identify any remaining gaps or uncertainties
4. Prepare final output with sources and confidence levels
```

### feature-development/SKILL.md
```markdown
---
name: feature-development
description: "End-to-end feature implementation with verification"
disable-model-invocation: true
---

# Full-Stack Feature Development

Implements a complete feature development workflow from planning to verification.
```

### feature-development/Workflows/full-stack-feature.md
```markdown
## Full-Stack Feature Development Workflow

### Stage 1: Planning (Planner Agent)
1. Clarify requirements and acceptance criteria
2. Break down into frontend/backend/database tasks
3. Identify dependencies and risks
4. Define verification methods for each component

### Stage 2: Architecture (Architect Agent)
1. Design component interfaces and data models
2. Choose appropriate patterns and technologies
3. Review for consistency with existing architecture
4. Create interface contracts

### Stage 3: Implementation (Parallel)
**Frontend Developer Agent**:
- Create UI components and hooks
- Implement client-side logic and state management
- Write unit tests for components

**Backend Developer Agent**:
- Implement API endpoints and business logic
- Design database schema and migrations
- Write unit and integration tests for APIs

### Stage 4: Integration
1. Connect frontend to backend APIs
2. Handle loading, error, and edge cases
3. Implement authentication and authorization flows
4. Run integration tests

### Stage 5: Verification (Tester Agent → Verification Loop)
1. Run full verification suite via verification-loop skill
2. If failures: route to appropriate specialist agents
3. Iterate until all verification passes
4. Update documentation and DECISIONS.md

### Stage 6: Review (Reviewer Agent)
1. Conduct comprehensive code review
2. Address all review comments
3. Final approval for merging
```

## 5. Hooks (`.claude/hooks/`)

### lint-on-edit.json
```json
{
  "matcher": "tool == \\\"Edit\\\" && tool_input.file_path matches \\\"\\\\\\\\.(ts|tsx|js|jsx)$\\\"",
  "hooks": [{
    "type": "command",
    "command": "npx eslint --fix \\\"$file_path\\\" && npx prettier --write \\\"$file_path\\\" && echo '[Hook] Linted and formatted $file_path'"
  }]
}
```

### test-on-save.json
```json
{
  "matcher": "tool == \\\"Edit\\\" && tool_input.file_path matches \\\"\\\\\\\\.(test|spec)\\\\.tsx?$\\\"",
  "hooks": [{
    "type": "command",
    "command": "vitest run \\\"$file_path\\\" --reporter=verbose && echo '[Hook] Ran tests for $file_path'"
  }]
}
```

### security-check.json
```json
{
  "matcher": "tool in [\\\"Edit\\\", \\\"Write\\\", \\\"NotebookEdit\\\"] && tool_input.file_path matches \\\"\\\\\\\\.(ts|tsx|js|jsx)$\\\"",
  "hooks": [{
    "type": "command",
    "command": "npx @typescript-eslint/eslint-plugin --plugin security --fix \\\"$file_path\\\" && echo '[Hook] Security check on $file_path'"
  }]
}
```

## 6. Commands (`.claude/commands/`)

### /research
```markdown
#!/usr/bin/env Skill("research")
When researching a topic:
1. Define clear research questions
2. Gather sources from multiple authorities
3. Synthesize findings with evidence
4. Validate conclusions
5. Return structured research report
```

### /build-feature
```markdown
#!/usr/bin/env Skill("feature-development")
To build a feature:
1. Invoke planner to break down work
2. Invoke architect for interface design
3. Run frontend and backend agents in parallel
4. Integrate components
5. Run verification loop until pass
6. Get final review approval
```

### /run-tests
```markdown
#!/usr/bin/env python3
import subprocess
import sys

def run_test_suite():
    print("Running full verification suite...")
    
    # Linting
    result = subprocess.run(["npm", "run", "lint"], capture_output=True, text=True)
    if result.returncode != 0:
        print("Linting failed:")
        print(result.stdout)
        return False
    
    # Unit tests
    result = subprocess.run(["npm", "run", "test:unit"], capture_output=True, text=True)
    if result.returncode != 0:
        print("Unit tests failed:")
        print(result.stdout)
        return False
    
    # Integration tests
    result = subprocess.run(["npm", "run", "test:integration"], capture_output=True, text=True)
    if result.returncode != 0:
        print("Integration tests failed:")
        print(result.stdout)
        return False
    
    # E2E tests
    result = subprocess.run(["npm", "run", "test:e2e"], capture_output=True, text=True)
    if result.returncode != 0:
        print("E2E tests failed:")
        print(result.stdout)
        return False
    
    print("All verification passed!")
    return True

if __name__ == "__main__":
    if not run_test_suite():
        sys.exit(1)
```

## 7. MCP Servers (`.claude/mcp/`)

### github.json
```json
{
  "version": "0.1.0",
  "name": "github-mcp",
  "description": "GitHub integration for Claude Code",
  "tools": [
    {
      "name": "create_issue",
      "description": "Create a GitHub issue",
      "inputSchema": {
        "type": "object",
        "properties": {
          "title": {"type": "string"},
          "body": {"type": "string"},
          "labels": {"type": "array", "items": {"type": "string"}}
        },
        "required": ["title", "body"]
      }
    },
    {
      "name": "get_pull_request",
      "description": "Get pull request details",
      "inputSchema": {
        "type": "object",
        "properties": {
          "pull_number": {"type": "number"}
        },
        "required": ["pull_number"]
      }
    }
  ]
}
```

### postgres.json
```json
{
  "version": "0.1.0",
  "name": "postgres-mcp",
  "description": "PostgreSQL database access",
  "tools": [
    {
      "name": "query",
      "description": "Execute a SQL query",
      "inputSchema": {
        "type": "object",
        "properties": {
          "sql": {"type": "string"},
          "parameters": {"type": "array", "items": {"type": ["string", "number", "boolean", "null"]}}
        },
        "required": ["sql"]
      }
    },
    {
      "name": "migrate",
      "description": "Run database migrations",
      "inputSchema": {
        "type": "object",
        "properties": {
          "direction": {"type": "string", "enum": ["up", "down"]},
          "steps": {"type": "number"}
        }
      }
    }
  ]
}
```

## 8. State Management Files

### DECISIONS.md
```markdown
# Project Decisions

## 2026-09-03: Authentication Approach
**Decision**: Use JWT with HttpOnly cookies for session management
**Why**: 
- Better security than localStorage (XSS protection)
- Automatic sending with requests
- Easy to implement on frontend and backend
**Alternatives considered**: 
- localStorage access tokens (rejected: vulnerable to XSS)
- Session IDs in DB (rejected: adds DB lookup overhead)
**References**: OWASP JWT cheat sheet, RFC 6750

## 2026-09-02: State Management
**Decision**: Use React Query for server state, Zustand for client state
**Why**:
- React Query handles caching, background updates, stale-while-revalidate
- Zustand is lightweight and easy to use for UI state
**Alternatives considered**: 
- Redux Toolkit (rejected: too verbose for our needs)
- SWR (rejected: less mature than React Query)
```

### claude-progress.md
```markdown
# Current Progress

## Authentication Feature (IN PROGRESS)
- [x] Backend: User registration endpoint
- [x] Backend: Login endpoint with JWT
- [ ] Backend: Password reset flow
- [x] Frontend: Registration form
- [x] Frontend: Login form
- [ ] Frontend: Protected routes
- [ ] Frontend: User profile page
- [ ] Unit tests: Auth services (60%)
- [ ] Integration tests: Auth flows (40%)
- [ ] E2E tests: Complete auth journey (20%)

**Next step**: Implement password reset backend
**Blocked by**: None
**Last updated**: 2026-09-03 14:30
```

### feature-list.json
```json
{
  "version": "1.0",
  "features": [
    {
      "id": "auth-system",
      "name": "User Authentication System",
      "description": "Complete user registration, login, and session management",
      "status": "active",
      "verification": "npm run test:auth",
      "dependencies": [],
      "estimated_hours": 16
    },
    {
      "id": "payment-processing",
      "name": "Payment Processing Integration",
      "description": "Stripe integration for subscription payments",
      "status": "not_started",
      "verification": "npm run test:payment",
      "dependencies": ["auth-system"],
      "estimated_hours": 20
    },
    {
      "id": "dashboard",
      "name": "User Dashboard",
      "description": "Personalized dashboard with user data and settings",
      "status": "not_started",
      "verification": "npm run test:dashboard",
      "dependencies": ["auth-system"],
      "estimated_hours": 12
    }
  ]
}
```

## How This Demonstrates Engineering Concepts

### Harness Engineering
- **Instructions**: CLAUDE.md files, AGENTS.md, skill descriptions
- **Tools**: Built-in file/web/execution tools + MCP servers (GitHub, PostgreSQL)
- **Environment**: Defined by project setup, Node.js version, package.json
- **State**: DECISIONS.md, claude-progress.md, feature-list.json, git commits
- **Feedback**: Verification loop, hooks, tester/reviewer agents, test suites
- **Orchestration**: Planner → Architect → Parallel implementation → Verification → Review

### Loop Engineering
- **Verification Loop**: Core test-fix-retry cycle in verification-loop skill
- **Research Loop**: Exploration → Deep dive → Synthesis → Validation in research skill
- **Development Loop**: Planning → Architecture → Implementation → Integration → Verification → Review
- **Micro-loops**: Lint fixing, test fixing, build fixing within verification loop
- **Human-in-the-loop**: Reviewer agent provides human oversight before merging

### Graph Engineering
- **Agent Graph**: 
  ```
  [Main Agent] 
      ↓
  [Planner] → [Architect] 
      ↓                     ↓
  [Frontend Dev] ← [Backend Dev] → [Tester] 
      ↓                     ↓             ↓
  [Verifier Loop]         [Integrator]  → [Reviewer]
      ↓
  [Done/Negative Feedback]
  ```
- **Skill Graph**: Skills as nodes with invocation edges based on context
- **Workflow Graph**: Feature development workflow as a directed graph
- **State Flow**: Information flows along edges (requirements → design → code → tests)

## Usage Examples

### Starting a New Feature
1. User: "/build-feature Implement user password reset functionality"
2. Claude Code:
   - Invokes planner to break down work
   - Invokes architect for API/data design
   - Delegates to frontend and backend agents in parallel
   - Runs verification loop after implementation
   - Gets reviewer approval before completion

### Fixing Bugs
1. User: "/fix-bug Login validation allows invalid emails"
2. Claude Code:
   - Locates relevant code via search
   - Creates failing test to reproduce issue
   - Runs verification loop to fix and verify
   - Ensures no regressions with full test suite

### Research Tasks
1. User: "/research Best practices for JWT implementation in HTTP-only cookies"
2. Claude Code:
   - Executes deep research workflow
   - Gathers authoritative sources (OWASP, RFCs, security blogs)
   - Synthesizes findings with evidence grading
   - Returns actionable recommendations

This harness provides a reliable, autonomous foundation for Claude Code to work effectively on complex software projects while maintaining quality and consistency.