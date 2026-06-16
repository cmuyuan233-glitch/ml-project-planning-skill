# Project Definition Brief Template

Use this as the first planning artifact. Keep it short enough for review, but precise enough to prevent premature modeling. The goal is to let the user think less: pick the suggested default when unsure and mark it as an assumption, rather than leaving a blank.

## 1. Business Decision

- User/team:
- Decision/action triggered by model:
- Automation level: analysis only / ranking / human review / automatic decision / policy support
- Business consequence of acting on the output:

## 2. Problem Framing

Decide the shape of the problem before anything else. Getting this wrong wastes the whole project.

- Is ML actually needed, or would a rule/heuristic do? (state the non-ML baseline)
- Problem type: classification / regression / ranking / forecasting / causal-uplift / retrieval / generation / anomaly / other
- Predict-vs-act check: if the output triggers an intervention, is the real question "who will do X" (predictive) or "who changes behavior if we act" (causal/uplift)?
- Modality: tabular / time-series / text / image / audio / multi-modal
- See `references/problem_framing.md` for the framing decision aids.

## 3. Prediction Unit

- One prediction row represents:
- Entity keys:
- Time/window keys:
- Required grain:

Examples:

- `user_id + prediction_date`
- `company_key + product_key + as_of_date`
- `order_id`
- `store_id + sku_id + forecast_week`

## 4. Prediction Timing

- When is the prediction made?
- What information is available at that time?
- Is the target current state, future behavior, or historical backfill?
- Required `as_of_date` or cutoff rule:

## 5. Model Output Contract

- Output type: probability / ranking score / class / regression value / calibrated risk
- Downstream consumer:
- Is business decision separate from model score? yes/no
- Required explanation or audit output:

## 6. Label Definition

- Positive label means:
- Negative label means:
- Unknown/unlabeled means:
- Review-only means:
- Label source of truth:
- Gold labels:
- Weak labels:
- Conflict policy:
- Do labels already exist, or must they be created? (if created, see the labeling plan in `references/data_requirements_spec.md`)

## 7. Error Cost

- False positive cost:
- False negative cost:
- Which is more expensive?
- Human review capacity or intervention cost:

## 8. Success Threshold & Expected Value

The kill/go bar. Without it, "good enough to ship" is undefined.

- Minimum metric value that makes the model worth deploying:
- Baseline to beat (current process, heuristic, or v1 model):
- Rough expected value if it works (revenue, cost saved, risk reduced):
- Cost/effort to build and run (qualitative is fine):
- What evidence would make us abandon or rescope the project?

## 9. Evaluation Priority

- Primary metric:
- Secondary metrics:
- Required slices:
- Calibration needed? yes/no
- Time/OOT evaluation needed? yes/no

## 10. Data Availability & Feasibility

ML lives or dies on data, and the data is often private or not yet collected. Capture the situation explicitly here, then expand it in `references/data_requirements_spec.md`.

- Data availability mode: `data_in_hand` / `data_partial` / `data_described` / `data_absent`
- Can the user share, only describe, or not yet provide the data?
- Rough label volume and positive base rate (real numbers, or a stated assumption):
- Is the labeled sample plausibly large enough? (yes / no / unknown -> `need_data`)
- Key assumptions that must hold for this to be feasible:

If data cannot be inspected, do not stop. Continue on stated assumptions and attach validation hooks.

## 11. Data Boundaries

- Allowed sources:
- Forbidden sources:
- No-database or no-network rules:
- Privacy/compliance restrictions:
- Fields blocked from model features:

## 12. Fairness & Compliance

- Protected or sensitive attributes present (directly or via proxies):
- Fairness slices to evaluate:
- Regulatory constraints (e.g. adverse-action/explanation, regional law):
- Is a human-readable reason for each decision required? yes/no

## 13. Non-Goals

List what this project will not solve in the first version.

## 14. Open Questions

| Question | Why It Matters | Status |
|---|---|---|

## 15. Readiness

- Ready for boundary register? yes/no
- Blocking missing information:

## Completion Contract

This brief is complete only when business decision, problem framing, prediction unit, prediction timing, label definition, error cost, success threshold, evaluation priority, data availability mode, data boundaries, fairness/compliance, non-goals, and open questions are explicit (or recorded as stated assumptions).

Do not move to feature planning or model planning if prediction unit, label truth, or prediction timing is still ambiguous. A missing data mode defaults to `data_described`; missing feasibility numbers become `need_data` assumptions, not blockers.
