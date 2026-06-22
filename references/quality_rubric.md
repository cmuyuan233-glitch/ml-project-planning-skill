# ML Planning Quality Rubric

This rubric scores outputs; `references/completion_contracts.md` is the canonical definition of the gates. Score each item 0, 1, or 2.

| Item | 0 | 1 | 2 |
|---|---|---|---|
| Business decision | Missing | Vague | Concrete user/action |
| Problem framing | Missing | Type only | ML-vs-heuristic + type + predictive/causal + modality |
| Prediction unit | Missing | Partially defined | Exact row grain and keys |
| Prediction timing | Missing | Mentioned | Clear cutoff/as_of_date |
| Label definition | Missing | Partial | Positive/negative/unknown sources clear |
| Label trust | Missing | Mentioned | Gold/weak/review-only separated |
| Error cost | Missing | Generic | FP/FN consequences and metric impact clear |
| Success threshold | Missing | Mentioned | Kill/go bar + baseline + rough value |
| Data availability | Missing | Implied | Mode chosen + feasibility + assumptions/hooks |
| Data safety boundary | Missing when real data is touched | Partial constraints | Data Safety Note or explicit N/A before data access |
| Leakage control | Missing | Some risks | Label/future/source/model leakage blocked |
| Evaluation protocol | Missing | Metrics only | Metrics + slices + calibration/time policy |
| Fairness/compliance | Missing | Mentioned | Slices + obligations, or explicit low-stakes N/A |
| Boundary register | Missing | Informal | fixed/data/EDA/experiment/deferred statuses |
| Feature plan | Missing | Feature list only | each feature has class/why/how/leakage + serving availability |
| Experiment matrix | Missing | Model list only | heuristic baseline + controlled variants and decision rules |
| Engineering architecture | Missing | High-level | layers, artifacts, manifests, serving parity, monitoring, gates |

## Interpretation

Scores scale with the number of items (max = 2 x number of items above).

- Below ~45% of max: Not ready for implementation.
- ~45-80% of max: Needs another planning pass.
- Above ~80% of max: Ready for architecture review.

## Failure Modes To Flag

- Jumps to model choice before defining labels.
- Uses accuracy without discussing error costs.
- Treats weak labels as gold.
- Treats evidence strength as label truth.
- Does not define prediction time.
- Does not separate model probability from business decision.
- Does not identify blocked leakage fields.
- Makes EDA/experiment decisions as hard-coded facts.
- Silently assumes data exists/is shareable instead of stating the data mode and assumptions.
- Inspects, profiles, exports, or uploads real data before the safety boundary is explicit.
- Frames an intervention problem as pure prediction when it is causal/uplift.
- Skips ML necessity / heuristic baseline.
- Ignores training-serving parity or post-deployment monitoring for a production model.

## Automatic Gate Failures

Regardless of score, mark the plan as not ready for implementation if any of these are true:

- prediction unit is missing;
- label definition is missing;
- prediction timing or `as_of_date` is missing for a time-dependent task;
- feature plan includes labels, review outcomes, future data, or downstream model scores as model inputs;
- calibration/test sampling policy is undefined for a classification/ranking project;
- model choice is finalized before split/evaluation protocol is defined;
- data is silently assumed (no availability mode and no assumptions register) when it was never inspected;
- real data is inspected, profiled, exported, or sent to external tools before a Data Safety Note or explicit approval boundary exists.
