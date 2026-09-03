---
title: Agent Loops
aliases:
  - Loop Patterns
  - Iterative Agent Behavior
tags:
  - foundations
  - agent-engineering
  - loop-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Agent Loops

## What are Agent Loops?

Agent loops are the iterative cycles through which AI agents pursue goals, improve their work, and adapt to feedback. Unlike one-shot interactions, loops enable agents to refine their outputs, correct mistakes, and progressively converge toward better solutions.

In the Agent = Model + Harness formula, loops represent how the harness structures repeated interactions with the model to achieve increasingly better results.

## Why Loops Matter More Than Single Interactions

### Limitations of One-Shot Interactions
- **No opportunity for correction**: Mistakes in the initial response cannot be fixed
- **Limited refinement**: No mechanism for iterative improvement
- **Fixed quality ceiling**: Output quality limited to what the model can produce in one pass
- **No learning from errors**: Agents cannot benefit from their own mistakes
- **All-or-nothing outcomes**: Either the first attempt works or it doesn't

### Advantages of Loop-Based Approaches
- **Error correction**: Mistakes can be identified and fixed in subsequent iterations
- **Quality progression**: Output quality can improve over multiple iterations
- **Adaptive refinement**: Agents can adjust their approach based on intermediate results
- **Confidence building**: Repeated success increases reliability
- **Resource efficiency**: Early termination when good enough solution is found
- **Exploration capability**: Ability to try different approaches and pivot when needed

## Core Components of Agent Loops

### 1. Planning Phase
**Determining what to do and how to approach it**

#### Activities:
- Goal clarification and refinement
- Approach selection and strategy formulation
- Resource and constraint assessment
- Subtask decomposition
- Success criteria definition

#### Outputs:
- Action plan or strategy
- List of required steps or subtasks
- Definition of done or acceptance criteria
- Resource allocation and timing estimates

### 2. Execution Phase
**Carrying out the planned actions**

#### Activities:
- Tool invocation and action taking
- Information gathering and processing
- Intermediate result generation
- Progress tracking and logging
- Adaptation to unexpected conditions

#### Outputs:
- Completed actions or work products
- Intermediate results and artifacts
- Progress indicators and metrics
- Observations and notes about the process

### 3. Observation Phase
**Examining what happened during execution**

#### Activities:
- Result inspection and analysis
- Output quality assessment
- Deviation detection from plans
- Feedback collection from environment
- Metric calculation and tracking

#### Outputs:
- Observation reports
- Quality measurements and metrics
- Deviations and anomalies identified
- Feedback from tools or environment
- Progress toward goals quantified

### 4. Verification Phase
**Determining if the work meets requirements**

#### Activities:
- Comparison against success criteria
- Rule and constraint checking
- Consistency and coherence validation
- External validation (tests, reviews, etc.)
- Defect and issue identification

#### Outputs:
- Pass/fail determination
- List of issues or defects found
- Verification evidence and documentation
- Severity and priority assessment of issues
- Recommendations for improvement

### 5. Correction Phase
**Addressing issues identified in verification**

#### Activities:
- Root cause analysis of problems
- Solution generation and selection
- Implementation of fixes or improvements
- Regression prevention measures
- Knowledge capture from correction process

#### Outputs:
- Corrected work products
- Fixes and improvements implemented
- Lessons learned documented
- Updated understanding or approach
- Preparation for next iteration

## Types of Agent Loops

### By Purpose

#### 1. Verification Loops
**Focus**: Ensuring correctness and quality
**Pattern**: Generate → Verify → (If fail) Correct → Repeat
**Example**: Write code → Run tests → If tests fail, fix code → Repeat

#### 2. Improvement Loops
**Focus**: Enhancing quality beyond basic correctness
**Pattern**: Generate → Evaluate → Enhance → Repeat
**Example**: Write draft → Assess clarity → Improve writing → Repeat

#### 3. Exploration Loops
**Focus**: Discovering information or solutions
**Pattern**: Search → Analyze → Identify gaps → Search deeper → Repeat
**Example**: Research topic → Find contradictions → Investigate further → Repeat

#### 4. Adaptation Loops
**Focus**: Adjusting to changing conditions or feedback
**Pattern**: Act → Observe environment → Adjust approach → Repeat
**Example**: Execute task → Notice user dissatisfaction → Modify approach → Repeat

#### 5. Learning Loops
**Focus**: Improving future performance based on past experience
**Pattern**: Act → Observe outcomes → Extract lessons → Update knowledge → Repeat
**Example**: Solve problem → Analyze what worked → Update problem-solving strategies → Repeat

### By Temporal Scope

#### 1. Inner Loops (Micro-loops)
- **Duration**: Seconds to minutes
- **Frequency**: High (many per task)
- **Purpose**: Immediate correction and refinement
- **Example**: Syntax checking while coding, spell checking while writing

#### 2. Outer Loops (Macro-loops)
- **Duration**: Minutes to hours or days
- **Frequency**: Low (few per task)
- **Purpose**: Strategic adjustment and major revisions
- **Example**: Complete feature implementation cycle, document revision cycle

#### 3. Episodic Loops
- **Duration**: Task-bound
- **Reset**: Begins fresh for each new task
- **Purpose**: Task-completion guarantee
- **Example**: "Keep working on this until it's done or you determine it's impossible"

#### 4. Continuous Loops
- **Duration**: Ongoing until explicitly stopped
- **Reset**: Persistent state across tasks
- **Purpose**: Long-term operation and service provision
- **Example**: Monitoring agent, automated response system

### By Control Mechanism

#### 1. Deterministic Loops
- Fixed number of iterations
- Predictable execution count
- Used when maximum effort is bounded
- Example: "Try up to 3 times to connect to the service"

#### 2. Conditional Loops
- Continue until condition met
- Variable execution count based on conditions
- Most common type in agent systems
- Example: "Keep improving until quality score exceeds threshold"

#### 3. Probabilistic Loops
- Continue with certain probability
- Used for exploration and randomized algorithms
- Example: "With 20% probability, try a completely different approach"

#### 4. Priority-Based Loops
- Work on highest priority items first
- Dynamic reprioritization during execution
- Example: "Always fix critical bugs before enhancements"

## Loop Engineering Principles

### 1. Design for Termination
Every loop must have clear, achievable termination conditions to prevent infinite execution.

#### Techniques:
- **Maximum iteration limits**: Hard bounds on loop repetitions
- **Quality thresholds**: Stop when output meets minimum standards
- **Resource exhaustion**: Stop when time, tokens, or budget exceeded
- **Convergence detection**: Stop when improvements become negligible
- **User intervention**: Allow manual termination

### 2. Make Progress Measurable
Loops require meaningful metrics to determine if improvement is occurring.

#### Metrics Types:
- **Quality metrics**: Correctness, completeness, clarity, etc.
- **Progress metrics**: Percentage completion, milestones reached
- **Efficiency metrics**: Time per iteration, resource utilization
- **Learning metrics**: Reduction in repeated errors, knowledge gain
- **Satisfaction metrics**: User feedback, stakeholder approval

### 3. Balance Exploration and Exploitation
Effective loops must balance refining known approaches with trying new ones.

#### Strategies:
- **Epsilon-greedy**: Mostly exploit best known approach, occasionally explore
- **Annealing**: Start with high exploration, gradually reduce to exploitation
- **Upper Confidence Bound**: Balance based on uncertainty and potential
- **Thompson Sampling**: Probabilistic approach based on belief distributions

### 4. Preserve Valuable Work
Avoid throwing away correct work when making corrections.

#### Techniques:
- **Incremental modification**: Change only what needs fixing
- **Regression testing**: Ensure fixes don't break previously working parts
- **Change isolation**: Limit scope of modifications to affected areas
- **Backup and rollback**: Ability to revert to previous good state

### 5. Learn from Each Iteration
Each loop cycle should improve the agent's future performance.

#### Mechanisms:
- **Experience storage**: Record what worked and what didn't
- **Pattern extraction**: Identify recurring issues and solutions
- **Strategy updating**: Adjust approaches based on accumulated experience
- **Knowledge distillation**: Convert specific lessons into general principles

## Common Loop Patterns in Agent Systems

### 1. Generate-Test-Fix (GTF) Loop
**Most common pattern for creation tasks**

```
Generate → Test → {If Pass: Done, If Fail: Analyze → Fix → Repeat}
```

**Applications**:
- Code generation with testing
- Writing with review and editing
- Design with validation and refinement
- Planning with feasibility checking

### 2. Research-Synthesis-Validation Loop
**Pattern for information gathering tasks**

```
Research → Synthesize → Validate → {If Adequate: Done, If Gaps: Research deeper → Repeat}
```

**Applications**:
- Technical research reports
- Market analysis and competitive intelligence
- Literature reviews and academic surveys
- Fact-checking and verification tasks

### 3. Plan-Act-Review Loop
**Pattern for execution and task completion**

```
Plan → Act → Review → {If Complete: Done, If Issues: Adjust Plan → Repeat}
```

**Applications**:
- Project execution and task management
- Multi-step workflows and procedures
- Goal-oriented behavior and achievement
- Adaptive processes in changing environments

### 4. Observe-Orient-Decide-Act (OODA) Loop
**Pattern for reactive and adaptive systems**

```
Observe → Orient → Decide → Act → (back to Observe)
```

**Applications**:
- Real-time monitoring and response
- Interactive assistants and chatbots
- Control systems and automation
- Adaptive interfaces and personalization

### 5. Explore-Expand-Distill Loop
**Pattern for knowledge building and understanding**

```
Explore → Expand → Distill → {If Understood: Done, If Confusion: Explore deeper → Repeat}
```

**Applications**:
- Learning new subjects and domains
- Building mental models and frameworks
- Complex problem solving and analysis
- Skill acquisition and mastery

## Loop Failure Modes and Anti-Patterns

### 1. Infinite Loops
**Symptom**: Loop never terminates, consuming resources indefinitely
**Causes**:
- Missing or incorrect termination conditions
- Unachievable success criteria
- State corruption preventing condition evaluation
**Prevention**:
- Always include maximum iteration limits
- Design verifiable termination conditions
- Implement timeout mechanisms
- Log iteration counts for debugging

### 2. Oscillating Loops
**Symptom**: Loop alternates between states without converging
**Causes**:
- Overcorrection (fixing too much each time)
- Conflicting feedback signals
- Missing middle ground in solution space
**Prevention**:
- Dampening factors in corrections
- Consensus or averaging mechanisms
- Intermediate state validation
- Exploration of solution space boundaries

### 3. Stagnant Loops
**Symptom**: Loop makes no progress despite iterations
**Causes**:
- Local maxima in optimization landscape
- Inadequate exploration mechanisms
- Feedback that doesn't guide improvement
**Prevention**:
- Periodic random exploration
- Simulated annealing or similar techniques
- Diverse initial conditions
- Meta-learning about loop effectiveness

### 4. Over-Refinement Loops
**Symptom**: Loop continues improving insignificant details past point of diminishing returns
**Causes**:
- Misaligned success criteria (optimizing wrong metrics)
- Perfectionism without cost-benefit analysis
- Lack of practical utility assessment
**Prevention**:
- Define meaningful quality thresholds
- Implement cost-benefit checking
- Include practical utility in success criteria
- Use time-boxing for perfectionist tendencies

### 5. Drifting Loops
**Symptom**: Loop gradually moves away from original goal or constraints
**Causes**:
- Success criteria that allow goal drift
- Feedback that rewards off-target improvements
- Lack of anchoring to original objectives
**Prevention**:
- Regular goal recommitment checks
- Constraint validation each iteration
- Original objective preservation
- Deviation detection and correction mechanisms

## Measuring Loop Quality

### 1. Convergence Rate
How quickly the loop reaches acceptable quality
- **Fast convergence**: Few iterations needed
- **Slow convergence**: Many iterations required
- **No convergence**: Quality doesn't improve over time
**Measurement**: Iterations to reach quality threshold

### 2. Stability
Consistency of outcomes across similar starting conditions
- **Stable**: Similar results from similar starts
- **Unstable**: Wildly different results from similar starts
**Measurement**: Variance in outcomes across multiple runs

### 3. Efficiency
Resources consumed per unit of quality improvement
- **Efficient**: Little resources for big improvements
- **Inefficient**: Many resources for small improvements
**Measurement**: Quality gain per token/time/resource unit

### 4. Robustness
Performance consistency across variations in inputs/conditions
- **Robust**: Works well across varied conditions
- **Fragile**: Breaks easily with minor changes
**Measurement**: Performance degradation under stress/test conditions

### 5. Generalizability
Ability to apply loop pattern to different but related tasks
- **Generalizable**: Same loop works for multiple task types
- **Specific**: Loop only works for one narrow task type
**Measurement**: Success rate when applying loop to task variations

## Loop Engineering in Claude Code

### Built-in Loop Mechanisms
Claude Code provides several native loop capabilities:

1. **Agentic Loop**: The default plan-act-observe-verify cycle
2. **Hook-Driven Loops**: Pre/post tool hooks can create verification cycles
3. **Skill Chaining**: Skills can invoke other skills in sequence
4. **Subagent Delegation**: Agents can delegate to subagents and wait for results
5. **Interactive Mode**: Continuous conversation enables natural looping

### Enhancing Loop Engineering in Claude Code

#### 1. Custom Verification Skills
Create skills that verify specific types of work:
```markdown
# Verify Frontend Component
When invoked, this skill will:
1. Check that the component follows React best practices
2. Verify accessibility compliance (WCAG 2.1 AA)
3. Confirm responsive design breakpoints
4. Validate prop types and default values
5. Return PASS/FAIL with specific feedback
```

#### 2. Hook-Based Verification Loops
Use hooks to create automatic verification:
- **Pre-tool hook**: Validate inputs before file modification
- **Post-tool hook**: Run tests after file edits
- **User input hook**: Prompt for confirmation on risky actions
- **Context enrichment hook**: Add relevant documentation before actions

#### 3. Structured Loop Skills
Design skills that embody specific loop patterns:
```markdown
# Research Loop Skill
This skill implements the Research-Synthesis-Validation loop:
1. Research Phase: Web search and source gathering
2. Synthesis Phase: Organize findings and identify gaps
3. Validation Phase: Check completeness and accuracy
4. If gaps found: Return to Research with focused queries
5. If adequate: Produce final report
```

#### 4. Progress Tracking Mechanisms
Build in ways to track loop progress:
- **claude-progress.md**: Update with each iteration
- **iteration counter**: Track how many times loop has run
- **quality metrics**: Record measurements from each verification
- **decision log**: Note why loop continues or terminates

#### 5. Timeout and Resource Management
Prevent runaway loops with safety mechanisms:
- **Maximum iteration counters** in skills
- **Time-based termination** using hooks
- **Token usage monitoring** and automatic termination
- **User intervention points** for long-running loops

### Claude Code Loop Patterns

#### 1. Test-Driven Development Loop
```
[Write failing test] 
    ↓
[Implement code to pass test] 
    ↓
[Run all tests] 
    ↓
[If any fail: Return to Implement, If all pass: Continue to next test]
```

#### 2. Research and Documentation Loop
```
[Gather sources] 
    ↓
[Create outline] 
    ↓
[Write sections] 
    ↓
[Verify completeness and accuracy] 
    ↓
[If gaps: Return to Gather with specific queries, If complete: Finalize]
```

#### 3. UI/UX Design Loop
```
[Create mockup] 
    ↓
[Check against design principles] 
    ↓
[Get feedback (simulated or real)] 
    ↓
[If issues: Return to Create with feedback, If approved: Finalize]
```

#### 4. Code Refactoring Loop
```
[Identify code smells] 
    ↓
[Apply refactoring technique] 
    ↓
[Run tests to ensure behavior unchanged] 
    ↓
[If tests fail: Return to Apply with fixes, If all pass: Look for next smell]
```

#### 5. Learning and Adaptation Loop
```
[Attempt solution] 
    ↓
[Evaluate outcome] 
    ↓
[Extract lessons from success/failure] 
    ↓
[Update internal knowledge or approach] 
    ↓
[Apply updated approach to similar future tasks]
```

## Best Practices for Effective Loops

### 1. Start Simple
Begin with the simplest loop that could possibly work, then add sophistication only when needed.

### 2. Make Termination Obvious
Ensure termination conditions are clear, testable, and impossible to misinterpret.

### 3. Verify Before Trusting
Never assume loop output is correct without explicit verification against defined criteria.

### 4. Keep Iterations Independent
Design iterations so that failure in one doesn't permanently corrupt the ability to succeed in later ones.

### 5. Log Everything
Detailed logging of each iteration enables debugging, analysis, and improvement of the loop mechanism itself.

### 6. Consider the Human Factor
Design loops that escalate to human intervention appropriately and provide clear handoff points.

### 7. Balance Automation and Judgment
Know when to automate completely and when to require human judgment in the loop.

### 8. Test Loops Themselves
Validate that your loop mechanisms work correctly with known good and bad inputs.

## Relationship to Other Engineering Paradigms

### With Harness Engineering
- Loops operate within the constraints and capabilities provided by the harness
- Harness provides the tools, context, and state that loops manipulate
- Effective harness design makes loops more powerful and easier to implement

### With Graph Engineering
- Simple loops are the basic building blocks of more complex graphs
- Graphs can be seen as collections of interconnected loops with defined transitions
- Loop outcomes often determine which path to take in a graph

### With Context Engineering
- Loops rely on context engineering to provide relevant information each iteration
- Context updates between iterations enable learning and adaptation
- Effective context engineering reduces the number of iterations needed

## Getting Started with Loop Engineering in Claude Code

### For Beginners
1. Understand the default agentic loop (plan-act-observe-verify)
2. Create a simple verification skill for your most common task type
3. Add a post-tool hook that runs your verification skill after file edits
4. Notice when the loop helps catch mistakes you would have missed

### For Intermediate Practitioners
1. Design custom loop skills for your domain (research, coding, writing, etc.)
2. Implement progress tracking in your loops (iteration counters, quality metrics)
3. Add safety mechanisms (timeouts, maximum iterations, user checkpoints)
4. Experiment with different loop patterns for different task types

### For Advanced Engineers
1. Build adaptive loops that change strategy based on performance
2. Create meta-loops that observe and improve the loop mechanisms themselves
3. Design loops that collaborate with human partners at key decision points
4. Implement learning loops that accumulate domain-specific expertise over time

## References and Further Reading

- [[Loop Engineering]] - For the broader topic of loop engineering in agent systems
- [[Harness Engineering]] - For how loops fit into the larger agent harness
- [[Context Engineering]] - For how context supports effective looping
- [[Agent Engineering]] - For foundational agent concepts
- [[Claude Code Architecture]] - For specific loop implementation in Claude Code
- [[Learning Roadmap]] - For structured progression in loop engineering skills

---