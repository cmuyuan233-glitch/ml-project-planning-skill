# ML Planning Quality Rubric

Score each item 0, 1, or 2.

| Item | 0 | 1 | 2 |
|---|---|---|---|
| Business decision | Missing | Vague | Concrete user/action |
| Prediction unit | Missing | Partially defined | Exact row grain and keys |
| Prediction timing | Missing | Mentioned | Clear cutoff/as_of_date |
| Label definition | Missing | Partial | Positive/negative/unknown sources clear |
| Label trust | Missing | Mentioned | Gold/weak/review-only separated |
| Error cost | Missing | Generic | FP/FN consequences and metric impact clear |
| Leakage control | Missing | Some risks | Label/future/source/model leakage blocked |
| Evaluation protocol | Missing | Metrics only | Metrics + slices + calibration/time policy |
| Boundary register | Missing | Informal | fixed/EDA/experiment/deferred statuses |
| Feature plan | Missing | Feature list only | each feature has class/why/how/leakage |
| Experiment matrix | Missing | Model list only | controlled variants and decision rules |
| Engineering architecture | Missing | High-level | layers, artifacts, manifests, implementation gates |

## Interpretation

- 0-10: Not ready for implementation.
- 11-18: Needs another planning pass.
- 19-24: Ready for architecture review.

## Failure Modes To Flag

- Jumps to model choice before defining labels.
- Uses accuracy without discussing error costs.
- Treats weak labels as gold.
- Treats evidence strength as label truth.
- Does not define prediction time.
- Does not separate model probability from business decision.
- Does not identify blocked leakage fields.
- Makes EDA/experiment decisions as hard-coded facts.

## Automatic Gate Failures

Regardless of score, mark the plan as not ready for implementation if any of these are true:

- prediction unit is missing;
- label definition is missing;
- prediction timing or `as_of_date` is missing for a time-dependent task;
- feature plan includes labels, review outcomes, future data, or downstream model scores as model inputs;
- calibration/test sampling policy is undefined for a classification/ranking project;
- model choice is finalized before split/evaluation protocol is defined.
