# Project Definition Brief Template

Use this as the first planning artifact. Keep it short enough for review, but precise enough to prevent premature modeling.

## 1. Business Decision

- User/team:
- Decision/action triggered by model:
- Automation level: analysis only / ranking / human review / automatic decision / policy support
- Business consequence of acting on the output:

## 2. Prediction Unit

- One prediction row represents:
- Entity keys:
- Time/window keys:
- Required grain:

Examples:

- `user_id + prediction_date`
- `company_key + product_key + as_of_date`
- `order_id`
- `store_id + sku_id + forecast_week`

## 3. Prediction Timing

- When is the prediction made?
- What information is available at that time?
- Is the target current state, future behavior, or historical backfill?
- Required `as_of_date` or cutoff rule:

## 4. Model Output Contract

- Output type: probability / ranking score / class / regression value / calibrated risk
- Downstream consumer:
- Is business decision separate from model score? yes/no
- Required explanation or audit output:

## 5. Label Definition

- Positive label means:
- Negative label means:
- Unknown/unlabeled means:
- Review-only means:
- Label source of truth:
- Gold labels:
- Weak labels:
- Conflict policy:

## 6. Error Cost

- False positive cost:
- False negative cost:
- Which is more expensive?
- Human review capacity or intervention cost:

## 7. Evaluation Priority

- Primary metric:
- Secondary metrics:
- Required slices:
- Calibration needed? yes/no
- Time/OOT evaluation needed? yes/no

## 8. Data Boundaries

- Allowed sources:
- Forbidden sources:
- No-database or no-network rules:
- Privacy/compliance restrictions:
- Fields blocked from model features:

## 9. Non-Goals

List what this project will not solve in the first version.

## 10. Open Questions

| Question | Why It Matters | Status |
|---|---|---|

## 11. Readiness

- Ready for boundary register? yes/no
- Blocking missing information:

## Completion Contract

This brief is complete only when business decision, prediction unit, prediction timing, label definition, error cost, evaluation priority, data boundaries, non-goals, and open questions are explicit.

Do not move to feature planning or model planning if prediction unit, label truth, or prediction timing is still ambiguous.
