# Responsible ML: Fairness, Compliance & Value

Use alongside the Project Definition Brief. Keep it lightweight: the point is to surface obligations early so they are designed in, not bolted on after a failed review.

## When this matters most

Escalate attention when the model affects people's access to money, work, housing, health, safety, or legal standing (credit, hiring, moderation, healthcare, insurance, justice). For internal/operational models the bar is lower but the checks still apply.

## Fairness

- Sensitive attributes present directly (age, gender, race, disability, etc.)?
- Proxy risk: do allowed features encode a sensitive attribute (zip code, device, name)?
- Fairness slices to evaluate (define the groups and the metric per group):
- Acceptable disparity definition and threshold:
- Mitigation options if a gap appears: reframe, reweight, post-process, or restrict features.

Add fairness slices to the evaluation protocol and to ongoing monitoring, not just to a one-time check.

## Compliance & explainability

- Regulatory regime (e.g. credit/ECOA adverse-action, GDPR explanation rights, sector law):
- Is a human-readable reason per decision required? If yes, model choice and feature choice are constrained.
- Data-use consent and retention limits:
- Audit trail required (who decided what, with which model version)?

## Expected value (kill/go bar)

Tie the project to a number so it can be stopped if it is not worth it.

- Minimum performance that makes deployment worthwhile (links to the Success Threshold in the brief):
- Rough expected value if it works:
- Build + run cost (qualitative is fine):
- Decision: proceed / rescope / stop.

## Completion Contract

Complete when sensitive attributes and proxies are identified, fairness slices and an acceptable-disparity definition exist for people-affecting models, explainability/compliance constraints are recorded, and the project has an explicit value bar. For low-stakes internal models, an explicit "low stakes, checks N/A because ..." note is acceptable.
