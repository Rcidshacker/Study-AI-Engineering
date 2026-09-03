---
title: Production Coding Agent
aliases:
  - Reliable Coding Agent Workflow
  - Production Agent System
tags:
  - scenario
  - production
  - ai-engineering
  - agent-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Production Coding Agent

## Problem Statement
"I want a reliable coding-agent workflow for a production application." This scenario addresses building a trustworthy, maintainable agent system suitable for professional software development in a production environment.

## Engineering Application

### Harness Engineering
- **Instructions**: Comprehensive CLAUDE.md with production standards, security policies, and compliance requirements
- **Tools**: Curated set of safe, verified tools via MCP servers (GitHub, Docker, Kubernetes, security scanners)
- **Environment**: Reproducible setup with exact version pinning, containerized dependencies, and validated build processes
- **State**: Persistent decision logs, audit trails, and version-controlled knowledge base
- **Feedback**: Multi-layer verification with automated gates, security scans, performance benchmarks, and compliance checks
- **Orchestration**: Defined agent roles, clear escalation paths, and human oversight checkpoints

### Loop Engineering
- **Verification Loops**: 
  - Layer 1: Linting + type checking + security scanning (on every edit)
  - Layer 2: Unit/integration tests + coverage requirements (80%+)
  - Layer 3: End-to-end user flows + performance benchmarks + security penetration testing
- **Maker-Checker Separation**: 
  - Implementation agent vs. verification agent (potentially different models)
  - Automated PR review bots as additional checkers
  - Human review for security-critical changes
- **Improvement Loops**: 
  - Retrospective analysis of agent failures to improve harness
  - Continuous learning from code review feedback
  - Adaptive verification based on historical failure patterns

### Graph Engineering
- **Agent Orchestration Graph**:
  ```
  [Product Manager] → [Architect] → [Tech Lead]
        ↓                   ↓              ↓
  [Frontend Team] ← [API Designer] → [Backend Team]
        ↓                   ↓              ↓
  [QA Lead] ← [Test Automation Engineer] ← [DevOps Engineer]
        ↓
  [Release Manager] → [Monitoring Engineer]
  ```
- **Skill/Capability Graph**: 
  - Nodes: Specialized skills (security, performance, compliance, etc.)
  - Edges: Prerequisites and compatibility relationships
  - Routing: Dynamic skill selection based on task analysis and risk assessment
- **State Flow Graph**: 
  - Requirements → Design → Implementation → Testing → Deployment → Monitoring
  - Feedback loops from monitoring to planning for continuous improvement
  - Emergency response paths for production incidents

## Claude Code Implementation

### Directory Structure
```text
production-app/
├── .claude/
│   ├── CLAUDE.md
│   ├── agents/
│   │   ├── product-manager.md
│   │   ├── architect.md
│   │   ├── tech-lead.md
│   │   ├── frontend-dev.md
│   │   ├── backend-dev.md
│   │   ├── qa-engineer.md
│   │   ├── devops-engineer.md
│   │   ├── security-specialist.md
│   │   └── release-manager.md
│   ├── skills/
│   │   ├── security-audit/
│   │   ├── performance-benchmark/
│   │   ├── compliance-check/
│   │   ├── verification-loop/
│   │   └── incident-response/
│   ├── hooks/
│   │   ├── security-pre-commit.json
│   │   ├── performance-post-deploy.json
│   │   └── compliance-pre-merge.json
│   ├── mcp/
│   │   ├── github-enterprise.json
│   │   ├── docker-registry.json
│   │   ├── kubernetes-cluster.json
│   │   ├── security-scanner.json
│   │   └── monitoring-service.json
│   └── commands/
│       ├── /deploy
│       ├── /security-audit
│       └── /incident-response
├── docs/
│   ├── ARCHITECTURE.md
│   ├── SECURITY-POLICY.md
│   └── COMPLIANCE-REQUIREMENTS.md
├── src/
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── security/
│   └── performance/
├── DECISIONS.md
├── claude-progress.md
├── feature-list.json
└── compliance-checklist.json
```

### Key Components

#### .claude/CLAUDE.md
```markdown
# Production Application - Claude Code Configuration

**Environment**: Production-grade web service handling PII and financial data
**Compliance**: SOC 2 Type II, GDPR, PCI DSS applicable components
**Security Standards**: 
- Zero trust architecture principle
- Defense in depth
- Least privilege access
- Secure by default
- Regular penetration testing

**Development Practices**:
- Trunk-based development with feature flags
- Immutable infrastructure
- Blue-green deployments
- Observability-first design
- ChatOps for incident response

**Verification Requirements**:
1. Security scan must pass (SAST/DAST)
2. Performance benchmarks within 5% of baseline
3. All tests pass with 90%+ coverage
4. Manual security review for auth/crypto changes
5. Compliance checklist completion

**Escalation Path**:
Agent → Tech Lead → Security Specialist → Architecture Board
```

#### Agent Definitions (examples)

##### .claude/agents/security-specialist.md
```markdown
# Security Specialist Agent

**Role**: Identify and mitigate security vulnerabilities
**When to invoke**: Security-sensitive code changes, vulnerability assessments, compliance audits
**Tools**: 
- Security scanner MCP
- Dependency checker
- Secret detection tools
- Cryptography validator
**Output**: Security report with severity ratings and remediation steps

## Workflow
1. Understand security context from CLAUDE.md and SECURITY-POLICY.md
2. Identify attack surfaces and threat models
3. Run automated security scans
4. Manual review of complex security logic
5. Generate remediation recommendations
6. Verify fixes through re-scanning
```

##### .claude/agents/devops-engineer.md
```markdown
# DevOps Engineer Agent

**Role**: Manage deployment, infrastructure, and observability
**When to invoke**: Release preparation, infrastructure changes, incident response
**Tools**: 
- Kubernetes MCP
- Docker registry MCP
- Monitoring service MCP
- Logging and tracing systems
**Output**: Deployment plan, rollback procedures, observability coverage

## Workflow
1. Review infrastructure-as-code changes
2. Validate deployment strategies
3. Check observability coverage (metrics, logs, traces)
4. Verify rollback procedures
5. Coordinate release timing with monitoring engineer
6. Post-deployment validation
```

#### Skills

##### .claude/skills/security-audit/SKILL.md
```markdown
---
name: security-audit
description: "Perform comprehensive security audit of codebase"
disable-model-invocation: true
---

# Security Audit Skill

Implements a thorough security scanning workflow.
```

##### .claude/skills/security-audit/Workflows/full-audit.md
```markdown
## Full Security Audit Workflow

### Phase 1: Static Analysis
1. Run SAST tools via security-scanner MCP
2. Analyze results for:
   - Injection vulnerabilities
   - Authentication bypasses
   - Authorization flaws
   - Cryptographic issues
   - Information leaks
3. Assign severity ratings (CVSS)
4. Generate remediation tickets for high/critical issues

### Phase 2: Dependency Scanning
1. Check all dependencies for known vulnerabilities
2. Identify outdated packages
3. Recommend updates with impact analysis

### Phase 3: Dynamic Analysis
1. Deploy to staging environment
2. Run DAST scans against running application
3. Test for runtime vulnerabilities
4. Validate security controls

### Phase 4: Manual Review
1. Security specialist review of complex logic
2. Threat modeling session
3. Penetration testing scope definition
4. Final security sign-off
```

#### Hooks

##### .claude/hooks/security-pre-commit.json
```json
{
  "matcher": "tool in [\\\"Edit\\\", \\\"Write\\\", \\\"NotebookEdit\\\"] && tool_input.file_path matches \\\"\\\\\\\\.(ts|tsx|js|jsx|py|java)$\\\"",
  "hooks": [{
    "type": "mcp",
    "server": "security-scanner",
    "tool": "scan_file",
    "arguments": {
      "file_path": "$file_path",
      "fail_on_high_severity": true,
      "block_commit": true
    }
  }]
}
```

##### .claude/hooks/compliance-pre-merge.json
```json
{
  "matcher": "tool == \\\"GitBranchMerge\\\"",
  "hooks": [{
    "type": "command",
    "command": "python .claude/scripts/check-compliance.py && echo '[Hook] Compliance check passed' || (echo '[Hook] Compliance check FAILED' && exit 1)"
  }]
}
```

#### Commands

##### .claude/commands/deploy
```markdown
#!/usr/bin/env Skill("devops-engineer")
To deploy to production:
1. Verify all security gates passed
2. Confirm performance benchmarks met
3. Ensure observability coverage
4. Execute blue-green deployment
5. Validate with smoke tests
6. Enable feature flags gradually
7. Monitor for 15 minutes before full cutover
```

### Why This Works

1. **Defense in Depth**: Multiple independent verification layers reduce single points of failure
2. **Clear Accountability**: Defined agent roles and escalation paths prevent diffusion of responsibility
3. **Continuous Verification**: Automated gates catch issues early in the development cycle
4. **Production-Ready Practices**: Incorporates established DevOps, SRE, and security methodologies
5. **Adaptability**: Feedback loops allow the system to improve based on real-world performance

### Failure Modes and Mitigations

#### Over-Automation Risk
- **Risk**: Too much automation leading to complacency
- **Mitigation**: Required human review for security-critical changes, regular manual penetration testing

#### Verification Bottlenecks
- **Risk**: Slow verification processes slowing development
- **Mitigation**: Parallel verification, incremental testing, smart test selection based on change impact

#### Alert Fatigue
- **Risk**: Too many false positives causing teams to ignore alerts
- **Mitigation**: Tuning verification thresholds, baselining normal behavior, focusing on actionable alerts

#### Context Overflow
- **Risk**: Too much information in CLAUDE.md reducing effectiveness
- **Mitigation**: Modular documentation with clear hierarchy, regular pruning of outdated information

#### Skill/Role Conflicts
- **Risk**: Overlapping responsibilities causing confusion
- **Mitigation**: Clear RACI matrices, regular role clarification sessions, escalation path documentation

## Adaptations for Different Contexts

### For Startups / MVP
- Reduce verification layers to essentials (lint → test → basic security)
- Use fewer specialized agents (combine roles)
- Simplify compliance requirements to core security practices
- Focus on speed of iteration with basic safety nets

### For Regulated Industries (Finance, Healthcare)
- Add additional verification layers (formal methods, third-party audits)
- Increase documentation and evidence requirements
- Implement stricter change control procedures
- Add specialized compliance officer agents

### For Open Source Projects
- Replace enterprise MCP servers with public equivalents
- Reduce specialization (community members wear multiple hats)
- Increase transparency in decision-making
- Focus on educational components in agent interactions

## Related Scenarios
- [[Multi Agent Development]] - For team-based agent collaboration
- [[Autonomous Test Fixer]] - For self-healing quality assurance
- [[Security-Focused Development]] - For enhanced security practices

---