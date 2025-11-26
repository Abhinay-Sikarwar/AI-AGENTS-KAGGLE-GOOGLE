# 03 — Context Engineering: Sessions & Memory

This module introduces **Context Engineering**, the discipline of dynamically constructing and managing the entire payload sent to an LLM at each conversational turn. Unlike static prompt engineering, Context Engineering is an *ongoing, adaptive* process that gives agents coherence, personalization, and long-term intelligence.

---

# 🔍 1. What Is Context Engineering?

### **Definition**
Context Engineering =  
**Dynamically assembling the RIGHT information inside the LLM’s context window at the RIGHT time.**

LLMs are **stateless**:
- Each API call has no memory of previous calls  
- No persistence between turns  
- No built-in session history  

Thus, agents must rebuild context on every request.

### Evolution
- **Prompt Engineering:** write static instructions  
- **Context Engineering:** dynamically build:
  - system prompts  
  - tool definitions  
  - retrieved memories  
  - session history  
  - external data  
  - few-shot examples  

---

# 🧠 2. Sessions — Short-Term Working Memory

A **session** is the temporary container for a single conversation.  
It represents the agent’s **short-term memory**.

### ✦ Components of a Session

#### 1. **Events**
Chronological log of:
- user messages  
- agent messages  
- tool calls  
- tool results  

#### 2. **State**
A structured working-memory scratchpad used for:
- workflow progress  
- task decomposition  
- temporary objects (cart items, steps left, partial plans)

### ✦ Why Sessions Exist
LLM execution environments are stateless, but agents must behave statefully.

Thus, **production agents save sessions in persistent storage**:
- Agent Engine Sessions  
- Redis  
- cloud session stores  

### ✦ The Session Problem: Context Rot

As conversation grows:
- cost ↑  
- latency ↑  
- noise ↑  
- attention ↓  
- reasoning quality ↓  

This is called **context rot**.

---

# 🔧 3. Session Compaction Strategies

To prevent context overload, agents must shrink sessions while preserving meaning.

### **1. Keep Last N Turns**
Simplest method; sliding window, but loses long-term coherence.

### **2. Token-Based Truncation**
Cut off conversation when a token limit is reached.

### **3. Recursive Summarization**
Most advanced method:
- older messages → replaced with LLM-generated summaries  
- triggered by token threshold or time  
- must run **asynchronously** to prevent slow responses  

This balances performance and coherence.

---

# 🧬 4. Memory — Long-Term User State

While sessions store *short-term turn history*,  
**memory stores distilled, persistent knowledge across sessions.**

Memory enables:
- personalization  
- continuity  
- preference retention  
- multi-session reasoning  

### ✦ Core Architecture

#### Memory Manager
A decoupled service (e.g., Agent Engine Memory Bank) that handles:
- storage  
- retrieval  
- consolidation  
- conflict resolution  

### ✦ Memory vs RAG

| Aspect | Memory | RAG |
|--------|--------|------|
| Purpose | User-specific personalization | Knowledge retrieval from documents |
| Data | Dynamic, private, isolated | Static or semi-static |
| Role | Makes agent *expert on the user* | Makes agent *expert on the world* |

They are complementary.

---

# 📚 5. Types of Memory

### **1. Declarative Memory** — “Knowing What”  
Facts about the user:
- name  
- preferences  
- background  
- stable attributes  

### **2. Procedural Memory** — “Knowing How”  
Skills & workflows:
- multi-step reasoning chains  
- tool sequences (e.g., how to book a flight)  
- personalized workflows  

---

# 🗂 6. Organizing Memory

### **A. Structured User Profile**
Key facts stored in structured fields.

### **B. Collections**
Multiple natural-language memories grouped by topic.

### **C. Rolling Summary**
Evolving summary of entire user history.

---

# 🔄 7. Memory Generation: LLM ETL Pipeline

Memory formation follows a 3-step Extract–Transform–Load pipeline.

### **1. Extraction**
LLM filters conversation for meaning:
- preferences  
- plans  
- constraints  
- new facts  

Uses:
- topic definitions  
- few-shot examples  
- guardrails  

### **2. Consolidation**
Most complex stage:
- compares new memory vs existing memory  
- resolves contradictions  
- deduplicates  
- updates or invalidates old entries  

Operations:
- **UPDATE**  
- **CREATE**  
- **DELETE / INVALIDATE**

### **3. Timing**
Memory generation should run:
- asynchronously  
- after the LLM responds  
- to avoid blocking user interaction  

---

# 🔎 8. Memory Retrieval & Injection

### Retrieval Scoring
Memories are selected based on:
- **relevance** (similarity search)  
- **recency**  
- **importance**  

### Memory Placement (Inference)
Memories are inserted into the LLM context by:

#### **1. System Instructions**  
For stable facts (identity, preferences).

#### **2. Conversation History**  
As tool outputs or injected messages.

### Memory-as-a-Tool
Agents use tools like:
- `create_memory`  
- `query_memory`  

This allows autonomous management of memory.

---

# 🔐 9. Privacy & Security

### Key Requirements
- Strict data isolation (user/tenant ACLs)
- PII redaction  
- Prevent memory poisoning  
- Sanitization + validation (Model Armor)  
- Removing sensitive tokens before storing memory  

Memory is the **most sensitive** and **attack-prone** part of agent architectures.

---

# 🧩 Summary

Context Engineering provides a formal approach to making agents:
- stateful  
- coherent  
- personalized  
- persistent  

Sessions provide **short-term** operational memory.  
Memory systems provide **long-term** continuity and intelligence.

Together, they enable truly **autonomous, user-aware AI agents**.