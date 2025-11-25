# 02 — Agent Tools & Interoperability with Model Context Protocol (MCP)

This module covers **how tools extend LLMs**, how agents use them to act in the world, and how the **Model Context Protocol (MCP)** standardizes tool integration across ecosystems.

---

# 🔍 1. Why Tools Matter

Foundation models are powerful but **passive**:
- They cannot access real-time data  
- They cannot interact with systems  
- They cannot perform actions  

**Tools give agents superpowers**:
- “Eyes” → access new information  
- “Hands” → perform actions  
- “Interfaces” → integrate with systems  

This transforms LLMs into **agentic systems capable of accomplishing goals**.

---

# 🧰 2. What Are Tools?

Tools = external functions or services the agent can call.

The course highlights **three classes**:

### **1. Function Tools (Developer-defined)**
- Code functions written in Python/JS/etc.  
- Documented via natural language docstrings  
- Agent reads their description → knows how to call them  
- Example:  
  - `create_jira_ticket`  
  - `set_light_values`  

### **2. Built-in Tools (Platform-provided)**
Examples from Google Gemini:
- **Grounding via Google Search**  
- **Code execution sandbox**  
- **URL content extraction**  
- **Computer Use tools**  

These tools are automatically available without user definition.

### **3. Agent Tools (Agents-as-tools)**
A primary agent can invoke a **sub-agent** to solve part of a task.

Patterns:
- Delegation  
- Hierarchical problem-solving  
- Expert subagents (math agent, planner agent…)

---

# 🧭 3. Tool Taxonomy by Purpose

Tools can be categorized by what they enable:

### 🔹 **Information Retrieval**
- Weather, stocks, news, SQL database queries  
- RAG sources, embeddings  

### 🔹 **Action Execution**
- Send emails  
- Write files  
- Update CRM  
- Modify calendar  

### 🔹 **System/API Integration**
- CRM, ERP, internal enterprise APIs  
- SaaS systems  

### 🔹 **Human-in-the-Loop**
Insert checkpoints:
- “Ask user to confirm”  
- “Request clarification”  

---

# 📝 4. Best Practices for Tool Design

A tool’s **design quality** determines an agent’s success.

### ✔ Clear names
**Good:**  
`create_critical_bug_with_priority`  
**Bad:**  
`update_jira`

### ✔ Describe **what** the tool does (not how)
Tell the agent:
- “Creates a high-priority bug ticket”
NOT:
- “Call this API with these params…”

### ✔ Publish high-level semantic tools  
Tools should represent tasks (not low-level API calls).

### ✔ Keep tools single-purpose  
One tool = One responsibility.

### ✔ Avoid dumping raw data  
Large datasets → overwhelm the LLM’s context window.  
Use:
- summaries  
- URIs  
- external storage  

### ✔ Strong error handling  
Return messages like:
- “Rate limit exceeded; retry in 15 seconds.”  
- “Invalid product name; ask user to clarify.”

LLMs can self-correct if errors are descriptive.

---

# 🔌 5. Interoperability with Model Context Protocol (MCP)

Tools become messy at scale:
- N agents × M tools = N×M integrations  
- Inconsistent schemas  
- No standard security  

**MCP solves this.**

### What is MCP?
A standard introduced by Anthropic (2024) to:
- Standardize tools  
- Standardize capabilities  
- Reduce integration overhead  
- Improve security  

### MCP Architecture

| Component | Role |
|----------|------|
| **Host** | UI / experience layer, controls tools, enforces security |
| **Client** | Runs inside Host, manages connection/session |
| **Server** | Exposes tools & capabilities; acts as adapter to external systems |

### MCP Communication
- Based on **JSON-RPC 2.0**
- Transport:  
  - `stdio` (local)  
  - `Streamable HTTP` (remote)

---

# 📦 6. MCP Tool Definition Format

Tools must specify:
- `name`  
- `description`  
- `inputSchema`  
- `outputSchema`  
- Optional `annotations` (hints like readOnly)

Outputs can be:
- **Structured JSON**  
- **Unstructured content** (text, audio, image)

Errors:
- JSON-RPC protocol errors  
- Tool execution errors via `"isError": true`

---

# 🛡 7. Why MCP Matters (Pros & Cons)

### ✔ Advantages
- Plug-and-play ecosystem  
- Faster tool development  
- Dynamic tool discovery  
- Decoupling of agent & tool implementation  
- Better governance potential  

### ⚠️ Challenges
- Context window bloat  
- Latency increase  
- No built-in auth standard  
- Stateful servers hard to scale  
- Sensitive data risk  

---

# 🔐 8. Security Challenges

Major risks include:

### 1. Dynamic Capability Injection  
Tool list can change silently.

### 2. Tool Shadowing  
Malicious tool overrides a legitimate one.

### 3. Malicious Tool Definitions or Output  
Prompt-injection inside tool descriptions.

### 4. Sensitive Information Leaks  
Tools might receive unintended user secrets.

### 5. No fine-grained access control  
Permissions not per-tool.

### 🔑 Mitigations
- Allowlists  
- mTLS  
- Input/output sanitization  
- Scoped short-lived credentials  
- Human-in-the-loop for critical actions  

---

# 📚 Summary

Tools make agents **useful**.  
MCP makes tools **interoperable, scalable, secure**.

This module explains how:
- Agents *act* using well-designed tools  
- MCP standardizes tool integration  
- Enterprises secure and govern agentic ecosystems  