# 🎧 Podcast Summary — Agent Quality

The episode argues that autonomous agents require a new approach to quality: built into their architecture and driven by observability and continuous evaluation.

---

## 1. Agent Non-Determinism → New QA Model
- Agents choose strategies; therefore, final-output tests are insufficient.
- Need to evaluate *how* agents decide and act.

## 2. Four Pillars of Quality
- Effectiveness (Goal Completion Rate)
- Efficiency (cost, latency, steps)
- Robustness (handle failures, replan)
- Safety & Alignment (ethical behavior, RAI)

## 3. The Strategic Framework
- **Outside-In:** Black-box end-to-end measurement.
- **Inside-Out:** Glass-box trajectory analysis (planning, tool use, interpretation).

## 4. Evaluators
- Automated metrics (ROUGE/BLEU/BERTScore) for trends.
- LLM-as-a-Judge and Agent-as-a-Judge for scalable qualitative scoring.
- HITL for gold-standard, domain-specific evaluation.
- RAI & red-teaming for safety.

## 5. Observability Is Essential
- Logs record events & intermediate reasoning.
- Traces connect those events into causal flows.
- Metrics summarize health & quality.

## 6. The Flywheel
- Define quality → Instrument → Evaluate → Convert failures to regression tests → Iterate.

---

## Final takeaway
Treat agent quality as a product-level, engineering-first concern — instrument thoroughly, evaluate continuously, and bake feedback into the system design.