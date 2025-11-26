# 📝 Whitepaper Summary — Context Engineering: Sessions & Memory

This document formalizes the discipline of dynamically constructing LLM context to enable stateful, personalized, long-lived AI agents.

---

# 1. Context Engineering: Definition & Role

### Core Idea
Context Engineering = assembling the **right set of tokens** for every LLM invocation.

LLMs are **stateless**; agents must manually build state through:
- system instructions  
- tool definitions  
- conversation history  
- memories  
- external data  

This is the next evolution beyond prompt engineering.

---

# 2. Sessions — Short-Term Conversation Container

### Components
- **Events:** user messages, assistant messages, tool calls/results  
- **State:** structured working memory for temporary agent reasoning  

### Persistence
Production systems must store session data externally because:
- LLMs cannot persist state  
- worker processes are ephemeral  

### Challenge: Context Rot
Large histories → degraded performance.

### Compaction Strategies
1. Keep last N turns  
2. Token-based truncation  
3. Recursive summarization  

Summarization is the most advanced and should run asynchronously.

---

# 3. Memory — Long-Term Knowledge

Memory stores distilled knowledge across sessions, enabling personalized agents.

### Architecture
Decoupled **Memory Manager** handles:
- extraction  
- consolidation  
- retrieval  
- storage  

### Memory vs RAG
- **Memory:** private, dynamic, per-user  
- **RAG:** external, factual, static knowledge  
Both are necessary.

---

# 4. Types of Memory

### Declarative Memory
User facts (knowing what).

### Procedural Memory
Workflow knowledge (knowing how).

### Organizational Patterns
- Structured user profile  
- Collections of memories  
- Rolling summary  

### Storage Options
- Vector databases (semantic search)  
- Knowledge graphs (relationships)  

---

# 5. Memory Generation — LLM ETL Pipeline

### 1. Extraction
Filter for meaningful new information using LLM prompts + guardrails.

### 2. Consolidation
Resolve contradictions, deduplicate, update or invalidate old facts.

### 3. Timing
Runs asynchronously after agent responds.

---

# 6. Memory Retrieval and Placement

### Retrieval Scoring:
- Relevance  
- Recency  
- Importance  

### Placement:
- System instructions → stable facts  
- Conversation history → dynamic facts  

### Memory-as-a-Tool:
Agents can autonomously:
- create new memories  
- query existing memories

---

# 7. Privacy & Security

To protect sensitive user data:
- strict ACL-based isolation  
- PII redaction  
- model armor against poisoning  
- sanitization of sensitive tokens before storage  

Memory is a high-risk component and must be tightly controlled.

---