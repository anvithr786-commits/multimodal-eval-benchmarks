# Evaluation Protocol (v0.1)

**Goal:** Produce repeatable, decision-useful evaluations for generative media models using a hybrid of human preference and assistive automated signals.

---

## 1) Benchmark sampling plan
### Task coverage
- Ensure coverage across task families (T1–T7).
- Use difficulty tiers: **easy / medium / hard** (balanced).

### Sampling rules
- Minimum per task family per run: **N = 20 prompts** (increase as capacity allows).
- Randomize prompt order and model order to reduce bias.
- Use the same prompt set for all compared models in a given run.

---

## 2) Human evaluation setup
### Format
- **Pairwise preference (A vs B)** as primary signal.
- Raters watch both clips fully **at least twice**.

### Raters & QA
- Minimum **2 raters per sample**.
- If disagreement on winner, either:
  - Add a **third rater**, or
  - Adjudicate via reason tags + rubric dimensions.

### Inter-rater agreement
Track:
- **% agreement** by task family
- Disagreement notes for the top 10 contentious samples

---

## 3) Scoring & aggregation
### Primary outputs
- Overall win-rate (A vs B)
- Win-rate by task family (T1–T7)
- Distribution of reason tags (top failure modes)

### Reporting rules
- Always include slice results (do not hide failures in overall averages).
- Highlight:
  - “Top 5 failure modes”
  - “Top 10 hardest prompts”
  - “Most improved task family” (if comparing iterations)

---

## 4) Automated assist signals (optional)
Automated signals are **assistive**, not definitive:
- Constraint checklist proxies (where machine-checkable)
- Stability/consistency heuristics (frame sampling)
- Identity similarity proxy (embedding-based)

Use automated signals mainly to:
- Triage failures
- Prioritize follow-up experiments
- Reduce manual review load

---

## 5) Release cadence (suggested)
- Weekly benchmark drops:
  - v0.1 → v0.2 prompt expansion
  - Add harder prompts over time
  - Maintain a changelog of task/prompt updates
