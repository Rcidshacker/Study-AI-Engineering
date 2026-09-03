---
title: Context Engineering
aliases:
  - Context Management
  - Information Access
tags:
  - foundations
  - agent-engineering
  - ai-engineering
status: evergreen
created: 2026-09-03
updated: 2026-09-03
---

# Context Engineering

## What is Context Engineering?

Context engineering is the practice of managing and optimizing the information provided to a language model to improve its performance, relevance, and reliability as part of an agent system. It focuses on ensuring that the model has access to the right information at the right time in the right format.

In the Agent = Model + Harness formula, context engineering primarily falls under the harness component, specifically dealing with how information is retrieved, filtered, formatted, and presented to the model.

## Why Context Engineering Matters

Language models have several limitations that context engineering helps address:

### 1. Finite Context Window
- Models can only process a limited amount of text (measured in tokens) at once
- Typical context windows range from 4K to 200K tokens depending on the model
- Beyond this limit, information is truncated or lost

### 2. Information Overload
- Providing too much irrelevant information can confuse the model
- "Lost in the middle" phenomenon where information in the center of long contexts is poorly attended to
- Signal-to-noise ratio affects model performance

### 3. Stale or Outdated Information
- Models have a knowledge cutoff date
- Rapidly changing information requires retrieval of current data
- Context must be updated for time-sensitive tasks

### 4. Irrelevant or Misleading Content
- Including incorrect or conflicting information can lead to hallucinations
- Poor context can cause models to reason from false premises
- Context quality directly impacts output quality

## Core Components of Context Engineering

### 1. Information Retrieval
**Getting the right information into the context window**

#### Techniques:
- **Keyword-based search**: Traditional search using terms and phrases
- **Vector similarity search**: Semantic search using embeddings
- **Hybrid search**: Combining keyword and vector approaches
- **Database queries**: Structured data retrieval from SQL/NoSQL sources
- **API calls**: Fetching real-time data from external services
- **File system traversal**: Reading relevant documents and code

#### Considerations:
- Relevance ranking and scoring
- Freshness and timeliness of information
- Access permissions and security
- Cost and latency of retrieval operations

### 2. Information Filtering and Selection
**Choosing what information to include in the context**

#### Techniques:
- **Relevance thresholding**: Only including items above a certain relevance score
- **Diversity optimization**: Ensuring varied perspectives and sources
- **Recency weighting**: Giving preference to newer information
- **Authority weighting**: Prioritizing trusted sources
- **Duplicate removal**: Eliminating redundant information
- **Length constraint optimization**: Fitting maximum value within token limits

#### Considerations:
- Balancing precision and recall
- Avoiding filter bubbles and echo chambers
- Maintaining completeness while respecting limits
- Handling conflicting information appropriately

### 3. Information Formatting and Presentation
**Structuring information for optimal model consumption**

#### Techniques:
- **Summarization**: Condensing long documents into key points
- **Chunking**: Breaking large documents into manageable pieces
- **Structured formats**: Using JSON, XML, YAML, or tables for clarity
- **Prompt templates**: Consistent formatting for different information types
- **Metadata inclusion**: Adding source, timestamp, reliability indicators
- **Hierarchical organization**: Grouping related information logically

#### Considerations:
- Token efficiency of different formats
- Model's ability to parse and utilize structure
- Preservation of important details during summarization
- Consistency in presentation for predictable model behavior

### 4. Context Window Management
**Dynamically controlling what resides in the model's active context**

#### Techniques:
- **Sliding windows**: Moving focus through long documents or conversations
- **Attention focusing**: Using model mechanisms to highlight relevant parts
- **External memory**: Storing information outside the context window
- **Recursive summarization**: Creating summaries of summaries for very long contexts
- **Context compression**: Removing redundancy while preserving meaning
- **Selective forgetting**: Removing outdated or irrelevant information

#### Considerations:
- Computational overhead of management techniques
- Latency introduced by context manipulation
- Preservation of long-range dependencies
- Tradeoffs between context size and model performance

### 5. Context Enrichment
**Enhancing information to improve model understanding**

#### Techniques:
- **Entity linking**: Connecting mentions to knowledge base entries
- **Relation extraction**: Identifying and expressing relationships between entities
- **Coreference resolution**: Linking pronouns and references to their antecedents
- **Disambiguation**: Clarifying ambiguous terms based on context
- **Expansion**: Adding related concepts or synonyms
- **Explanation generation**: Providing reasoning steps or justifications

#### Considerations:
- Accuracy of enrichment techniques
- Potential for introducing errors through incorrect enrichment
- Computational cost of enrichment processes
- Balance between raw information and interpreted enhancements

## Context Engineering Strategies

### 1. Retrieval-Augmented Generation (RAG)
The most common context engineering pattern:
1. Receive query or task description
2. Retrieve relevant information from external sources
3. Format and insert retrieved information into the prompt
4. Generate response using model with augmented context

**Variations**:
- **Naive RAG**: Simple retrieval and insertion
- **Advanced RAG**: With reranking, query transformation, etc.
- **Graph RAG**: Using knowledge graphs for retrieval and reasoning
- **Iterative RAG**: Multiple retrieval and generation passes
- **Self-RAG**: Model critiques and improves its own retrieval

### 2. Fine-tuning vs. Context Engineering
When to use each approach:

**Context Engineering Preferred When**:
- Information changes frequently
- Need for up-to-date, real-time data
- Want to avoid retraining costs
- Require interpretability and traceability
- Dealing with domain-specific or proprietary information

**Fine-tuning Preferred When**:
- Consistent, stable domain knowledge
- Need for specialized behavior or tone
- High-volume, repetitive similar tasks
- Latency requirements favor pre-compiled knowledge
- Want to reduce per-invocation context size

### 3. Hierarchical Context Approaches
Managing information at different levels of detail and urgency:

- **Working Context**: Immediate information in model's context window (highest priority)
- **Recent Context**: Immediately accessible external memory (e.g., conversation history)
- **Working Memory**: Frequently accessed information in fast storage
- **Long-term Memory**: Infrequently accessed but important knowledge
- **Archival Context**: Rarely accessed reference material

### 4. Adaptive Context Strategies
Dynamically adjusting context based on task and feedback:

- **Feedback-Driven Adjustment**: Using output quality to inform context selection
- **Uncertainty-Based Retrieval**: Seeking more information when confidence is low
- **Task-Adaptive Context**: Different strategies for different task types (coding vs. writing vs. analysis)
- **Resource-Aware Context**: Adjusting based on available computational budget
- **Learning from Corrections**: Improving context selection based on past mistakes

## Context Engineering in Agent Systems

### Relationship to Other Engineering Paradigms

#### With Harness Engineering
Context engineering is a specialized subset of harness engineering focused specifically on information management. Other harness components include:
- Tools and permissions (what the agent can do)
- Instructions and constraints (how the agent should behave)
- State and memory (persistence across time)
- Feedback and verification (knowing when it's right)
- Orchestration (coordinating complex behavior)

#### With Loop Engineering
Context engineering supports effective loops by:
- Providing relevant information for each iteration
- Enabling learning from previous iterations through context updates
- Supporting verification with appropriate evidence and criteria
- Facilitating adaptation based on iterative outcomes

#### With Graph Engineering
In multi-agent or workflow systems, context engineering manages:
- Information flow between nodes in a graph
- State sharing and synchronization mechanisms
- Local vs. global context distribution
- Context partitioning for parallel execution
- Merging and resolving conflicting context information

## Practical Context Engineering Techniques

### 1. Context Window Optimization
- **Prioritization algorithms**: Score and rank information by relevance and importance
- **Compression techniques**: Remove redundancy while preserving meaning (e.g., LEAN, LLMLingua)
- **Incremental context building**: Add information gradually as needed
- **Context window expansion**: Use models with larger native contexts when available
- **External context stores**: Keep most information outside, bring in only what's needed

### 2. Retrieval Strategies
- **Dual-encoder retrieval**: Separate models for query and passage encoding
- **Cross-encoder reranking**: Use more powerful model to re-rank initial results
- **Query transformation**: Rewrite or expand queries for better retrieval
- **Hybrid search**: Combine sparse (BM25) and dense (vector) retrieval
- **Metadata filtering**: Use structured metadata to pre-filter candidates

### 3. Formatting for Model Consumption
- **Clear delimiters**: Use consistent markers to separate different information types
- **Structured prompts**: Use JSON, YAML, or XML formats when beneficial
- **Instruction-context separation**: Clearly distinguish between what to do and what to work with
- **Example inclusion**: Provide few-shot examples when helpful
- **Chain-of-thought preparation**: Format information to facilitate reasoning steps

### 4. Managing Conversation Context
- **Summarization strategies**: Periodically condense long conversations
- **Topic segmentation**: Identify and separate different discussion topics
- **Importance weighting**: Preserve key decisions and action items
- **Sliding windows**: Maintain fixed-size window of most recent exchange
- **Memory injection**: Retrieve relevant parts of long conversations as needed

### 5. Code-Specific Context Engineering
- **Repository mapping**: Understand codebase structure and dependencies
- **Symbol resolution**: Connect references to their definitions
- **Change tracking**: Focus on recently modified or relevant code sections
- **Dependency graphs**: Understand impact of changes
- **Test-informed context**: Use test files to understand expected behavior
- **Documentation integration**: Include relevant API docs and comments

## Context Engineering Failure Modes

### 1. Retrieval Failures
- **False negatives**: Missing relevant information that should have been retrieved
- **False positives**: Including irrelevant or misleading information
- **Stale information**: Retrieving outdated data when current information is needed
- **Source bias**: Over-reliance on particular sources or perspectives

### 2. Formatting and Presentation Failures
- **Token inefficiency**: Wasting context window on formatting overhead
- **Information loss**: Losing important details during summarization or compression
- **Confusing structure**: Presenting information in ways that hinder model understanding
- **Inconsistent formatting**: Making it difficult for model to parse and utilize context

### 3. Context Management Failures
- **Context overflow**: Exceeding model's context window limits
- **Context starvation**: Providing insufficient information for the task
- **Stale context**: Holding onto outdated information too long
- **Premature eviction**: Removing useful information too early

### 4. Enrichment and Reasoning Failures
- **Incorrect enrichment**: Adding wrong information through entity linking or relation extraction
- **Over-enrichment**: Adding so much interpretation that it obscures raw information
- **Under-enrichment**: Failing to disambiguate or clarify when needed
- **Reasoning errors**: Making incorrect inferences during context enhancement

## Context Engineering in Claude Code

### Built-in Context Mechanisms
Claude Code provides several context management features:

1. **Automatic Context Loading**:
   - CLAUDE.md files from working directory and parent directories
   - System prompt and built-in instructions
   - Skill descriptions when relevant

2. **Context Window Management**:
   - Automatic compaction when context window fills
   - Preservation of recent messages in full
   - Summarization of older content

3. **Tool-Driven Context**:
   - File read operations bring file contents into context
   - Web search results added to context
   - Code execution outputs available for reasoning
   - MCP server responses integrated into context

### Enhancing Context Engineering in Claude Code

#### 1. Optimizing CLAUDE.md Usage
- **Modular organization**: Split into multiple focused files
- **Strategic placement**: Critical constraints at top and bottom
- **Referential approach**: Link to detailed documentation rather than duplicating
- **Regular pruning**: Remove outdated or irrelevant content
- **Layered detail**: High-level overview with available deep dives

#### 2. Strategic Skill Invocation
- **Relevance-based loading**: Skills load only when contextually appropriate
- **Frontmatter tuning**: Improve skill discovery through better descriptions
- **Parameterization**: Make skills adaptable to different contexts
- **Chaining and composition**: Combine skills for richer context

#### 3. Tool Selection and Usage
- **Targeted file reads**: Read only necessary parts of large files
- **Strategic web search**: Use search to fill specific knowledge gaps
- **Purposeful code execution**: Execute code to generate needed insights
- **Efficient MCP usage**: Fetch only required data from external services

#### 4. Context State Persistence
- **claude-progress.md**: Track current state and progress
- **DECISIONS.md**: Preserve important conclusions and rationales
- **Feature lists**: Machine-readable progress tracking
- **Git commits**: Use as contextual checkpoints

#### 5. Hook-Driven Context Management
- **Pre-tool hooks**: Prepare and validate context before tool use
- **Post-tool hooks**: Process and integrate tool results into context
- **Context enrichment hooks**: Add metadata, summaries, or related information
- **Context filtering hooks**: Remove irrelevant or redundant information

### Claude Code Context Engineering Patterns

#### 1. Research-Enhanced Context
```
[Initial Query] 
    ↓
[Web Search for Background] 
    ↓
[Read Key Sources] 
    ↓
[Summarize and Integrate] 
    ↓
[Focused Search for Details] 
    ↓
[Integrate Findings] 
    ↓
[Generate Response with Rich Context]
```

#### 2. Codebase-Aware Context
```
[Task Description] 
    ↓
[Identify Relevant Files] 
    ↓
[Read Key Functions/Classes] 
    ↓
[Understand Dependencies] 
    ↓
[Review Related Tests] 
    ↓
[Generate Implementation with Full Context]
```

#### 3. Iterative Refinement Context
```
[Attempt Solution]
    ↓
[Evaluate Against Criteria]
    ↓
[Identify Gaps/Misunderstandings]
    ↓
[Retrieve Targeted Information]
    ↓
[Update Context with New Understanding]
    ↓
[Refine Solution]
```

## Best Practices and Guidelines

### 1. Start with Clear Goals
- Define what information is truly needed for the task
- Distinguish between essential, helpful, and irrelevant information
- Consider different information types (factual, procedural, contextual, examples)

### 2. Measure and Monitor Context Usage
- Track context window utilization
- Monitor token consumption by information type
- Measure retrieval latency and effectiveness
- Evaluate impact of context changes on output quality

### 3. Experiment and Iterate
- Try different retrieval strategies and compare results
- Test various formatting approaches
- A/B test context management techniques
- Learn from failures and successes

### 4. Prioritize Precision Over Recall
- It's generally better to have less but more relevant information
- Missing information can often be retrieved in subsequent iterations
- Irrelevant information actively harms performance
- Err on the side of inclusion only when cost of missing is very high

### 5. Maintain Context Hygiene
- Regularly review and update information sources
- Remove outdated, incorrect, or misleading content
- Ensure consistency between different context sources
- Validate that context actually helps rather than hurts

## Tools and Techniques for Context Engineering

### Retrieval Systems
- **Traditional Search**: Elasticsearch, Solr, basic text search
- **Vector Databases**: Pinecone, Weaviate, Qdrant, Milvus, FAISS
- **Hybrid Search**: Combining keyword and vector approaches
- **Specialized Engines**: Domain-specific search tools (code, legal, medical, etc.)

### Context Management Libraries
- **LangChain**: Context management, retrieval, and augmentation chains
- **LlamaIndex**: Data indexing and querying for LLMs
- **Haystack**: Open-source framework for building search systems
- **Embedding APIs**: OpenAI, Cohere, Hugging Face for vector generation

### Summarization and Compression
- **Extractive Summarization**: Selecting key sentences (TextRank, LexRank)
- **Abstractive Summarization**: Generating new summaries (BART, T5, PEGASUS)
- **Context Compression**: Specialized techniques like LLMLingua, RECOMP
- **Chunking Strategies**: Fixed-size, semantic, or agent-based chunking

### Evaluation and Measurement
- **Context Utilization Metrics**: Percentage of context actually used
- **Relevance Scoring**: Human or automated assessment of retrieved information
- **Output Quality Metrics**: Accuracy, relevance, coherence of generations
- **Efficiency Measures**: Tokens used per unit of output quality

## Relationship to Related Disciplines

### Context Engineering vs. Prompt Engineering
- **Prompt Engineering**: Focuses on crafting the input (prompt) to the model
- **Context Engineering**: Focuses on managing the information available to the model
- **Overlap**: Both involve preparing information for model consumption
- **Distinction**: Prompt engineering is about *how* you ask; context engineering is about *what* you show the model

### Context Engineering vs. Retrieval-Augmented Generation (RAG)
- **RAG**: A specific technique that combines retrieval with generation
- **Context Engineering**: Broader discipline encompassing RAG and other approaches
- **RAG as Subset**: RAG is one important method within context engineering

### Context Engineering vs. Fine-tuning
- **Context Engineering**: Provides information at inference time without changing model weights
- **Fine-tuning**: Modifies model weights to embed knowledge directly
- **Tradeoffs**: Context engineering offers flexibility and freshness; fine-tuning offers speed and specialization

### Context Engineering vs. Memory Systems
- **Context Engineering**: Manages information for immediate model consumption
- **Memory Systems**: Handle long-term storage, retrieval, and organization of knowledge
- **Relationship**: Memory systems feed into context engineering processes

## Future Directions in Context Engineering

### 1. Dynamic Context Allocation
- Systems that automatically adjust context window allocation based on task demands
- Priority-based context management with preemption
- Predictive context loading based on anticipated needs

### 2. Multi-Modal Context Engineering
- Managing context across text, images, audio, video, and other modalities
- Cross-modal retrieval and reasoning
- Unified context spaces for heterogeneous information

### 3. Causal Context Engineering
- Focusing on causally relevant information rather than just correlational
- Interventional context modification to test hypotheses
- Context engineering that supports causal reasoning and counterfactuals

### 4. Personalized Context Engineering
- Tailoring context selection and presentation to individual user preferences
- Learning individual patterns of information usefulness
- Adaptive systems that improve context engineering per user over time

### 5. Context Engineering for Agent Societies
- Managing shared, private, and public context in multi-agent systems
- Context marketplaces where agents buy and sell information
- Reputation systems for context providers
- Context norms and protocols for agent societies

## Getting Started with Context Engineering

### For Beginners
1. Understand the token limits of your model
2. Experiment with different amounts of context and observe effects
3. Try simple retrieval (e.g., grep/followed by manual selection) for specific tasks
4. Practice summarizing long documents to fit within context limits
5. Notice how different types of information affect model outputs

### For Intermediate Practitioners
1. Implement a basic RAG pipeline for your domain
2. Experiment with different retrieval strategies (keyword vs. vector vs. hybrid)
3. Practice context window management techniques
4. Learn to measure and monitor context effectiveness
5. Start building reusable context engineering components

### For Advanced Engineers
1. Design sophisticated adaptive context systems
2. Implement multi-stage retrieval and refinement processes
3. Build context engineering systems that learn from feedback
4. Create personalized context engineering approaches
5. Push the boundaries with multi-modal and causal context engineering

## References and Further Reading

- [[Harness Engineering]] - For the broader context of agent scaffolding
- [[Agent Engineering]] - For foundational agent concepts
- [[Loop Engineering]] - For how context supports iterative improvement
- [[Graph Engineering]] - For context in multi-agent systems
- [[Claude Code Architecture]] - For specific context management in Claude Code
- [[Learning Roadmap]] - For structured progression in context engineering skills

---