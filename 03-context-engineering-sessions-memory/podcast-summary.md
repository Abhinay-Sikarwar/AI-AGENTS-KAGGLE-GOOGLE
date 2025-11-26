# 🎧 Podcast Summary — Context Engineering: Sessions & Memory

This episode explains how to build stateful agents despite LLMs being stateless by default.

---

# 1. Context Engineering Basics

### LLM Weakness:
LLMs do **not remember anything** across calls.

### Solution:
Dynamically build the entire context window each turn using:
- instructions  
- history  
- examples  
- external data  
- tool definitions  
- memories  

This is the evolution from **prompt engineering → context engineering**.

---

# 2. Sessions: Short-Term Working Memory

A session contains:
- **Events** — the chronological conversation history  
- **State** — structured working memory  

### Challenges:
As sessions grow, the model:
- becomes slower  
- loses focus  
- suffers “context rot”  

### Fix:
Compaction strategies like:
- sliding window  
- token-based truncation  
- recursive summarization (LLM-generated summaries)

Compaction must run asynchronously.

---

# 3. Memory: Long-Term Persistence

Memory persists across sessions and supports personalization.

### Memory vs RAG
- **RAG** → external, factual knowledge  
- **Memory** → user-specific, dynamic knowledge  
Both are complementary.

### LLM-driven ETL pipeline:
1. **Extraction**  
2. **Consolidation**  
3. **Storage** (asynchronously)

### Retrieval uses:
- relevance  
- recency  
- importance  

Memories can be inserted via:
- System instructions  
- Conversation history  

Agents can also use Memory Tools like:
- `query_memory`
- `create_memory`

---

# 4. Security Considerations
- isolate user data  
- redact PII  
- avoid memory poisoning  
- sanitize inputs/outputs  

---

# 🎙️ Key Takeaway
LLMs alone are stateless.  
Agents become intelligent when equipped with well-managed **sessions** and **memory**.