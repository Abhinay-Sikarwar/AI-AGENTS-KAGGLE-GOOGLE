# 04 — Agent Quality

This module treats **agent quality** as an architectural requirement — not a late-stage test. It presents a framework for designing, measuring, and iterating on autonomous agents using observability, trajectory-aware evaluation, and continuous improvement.

---

## 🔑 Core Principle

**Agent quality is architectural.**  
Because agents are non-deterministic and choose *how* to accomplish goals, quality must be engineered into the system (design, observability, evaluation) rather than merely tested at the end.

---

## 🧾 The Four Pillars of Agent Quality

1. **Effectiveness (Goal Achievement)**  
   - Primary measure: **Goal Completion Rate (GCR)** — whether the agent completes the intended mission.
   - Also track decomposed task metrics for multi-step goals.

2. **Efficiency (Operational Cost & Latency)**  
   - Tokens per task, wall-clock latency (P50/P99), action path length (number of steps).
   - Lower cost & shorter, correct trajectories are preferable.

3. **Robustness (Reliability & Graceful Failure)**  
   - Handles API timeouts, ambiguous inputs, partial failures; can replan or fallback.

4. **Safety & Alignment (Trustworthiness)**  
   - Ethical constraints, refusal behaviors, prompt-injection resistance, and RAI metrics.

---

## 🧭 Evaluating Agents — Beyond Final Output

### The Trajectory Is The Truth
- Evaluate the whole **think → act → observe** trajectory, not just the final answer.
- Use a reference/ideal trajectory to compute precision & recall on tool usage and steps.

### Key Trajectory Metrics
- **Precision (Tool correctness):** Fraction of agent-invoked tool calls that were relevant/expected.
- **Recall (Completeness):** Fraction of required tool calls that the agent actually invoked.
- **Trajectory Length:** Number of steps — shorter when efficient, longer if needed for robustness.
- **Deviation:** Points where the agent diverged from an expected path (useful for debugging).

---

## 🧰 Evaluation Methods

1. **Outside-In (Black Box)**  
   - End-to-end success metrics (GCR, user satisfaction, business KPIs).

2. **Inside-Out (Glass Box)**  
   - Trace the execution path: planning quality, tool selection, interpretation of tool outputs, RAG quality, and error handling.

3. **Automated Judging**  
   - **LLM-as-a-Judge:** Use an LLM to score outputs against a rubric (pairwise comparisons to reduce bias).
   - **Agent-as-a-Judge:** Use specialized agents to evaluate other agents' traces.

4. **Human-in-the-Loop (HITL)**  
   - Gold-standard labeling, nuanced judgments, red-team and safety validation.

---

## 📈 Observability — The Foundation

Three pillars provide the data needed to evaluate and debug agents:

1. **Logging**  
   - Structured logs (JSON), timestamped, recording events, intermediate reasoning, tool calls, and tool results.

2. **Tracing**  
   - Connect logs into spans to show causal flow (why the agent took each step).

3. **Metrics**  
   - System metrics: latency (P50/P99), error rates, tokens per task.
   - Quality metrics: goal success rates, trajectory adherence, judge scores.

---

## 🔁 The Agent Quality Flywheel (Continuous Improvement)

1. **Define quality** (pillars & KPIs).  
2. **Instrument** (logs, traces, metrics).  
3. **Evaluate** (outside-in → inside-out, LLM/agent judges, HITL).  
4. **Feedback & Regression** — turn failures into regression tests in a Golden evaluation set.  
Repeat.

---

## 🛡 AgentOps Practical Considerations

- **Trace every step**: reasoning, tool call, tool result.  
- **Store golden traces** for regression tests.  
- **Automate early gates**: run automated judges in CI, invoke HITL for critical failures.  
- **Design tools for observability**: add correlation IDs, structured error types, and clear docstrings.  
- **Privacy & Safety**: ensure logs do not leak PII and that RAI checks are part of the CI gate.

---

## 🧩 Summary

Agent quality requires instrumented systems that measure trajectories, not just outputs. By combining observability, automated & human evaluation, and a continuous feedback loop (the flywheel), teams can produce reliable, safe, efficient, and effective agents suitable for production.