# Problem Framing Aid

Run this before defining labels, features, or models. A wrong frame cannot be fixed later by a better model. The aim is to let the user pick a frame quickly using defaults and short decision questions, not to write an essay.

## 1. Do you even need ML?

Prefer the simplest thing that meets the success threshold.

- Is there a known rule or formula that already works well enough? Use it; skip ML.
- Can a deterministic heuristic serve as the baseline to beat? Define it now; every model must beat it.
- Is the relationship stable, low-volume, or fully specified by policy? ML may add cost without value.

Default: always define a non-ML baseline, even if you expect to use ML. It anchors the success threshold and the experiment matrix.

## 2. Predict vs act (the causal/uplift trap)

If the model output triggers an action, ask what question you are really answering.

- Predictive: "What will happen if we do nothing?" (e.g. who will churn) — standard supervised learning.
- Causal / uplift: "Who changes behavior because we act?" (e.g. who to send a retention offer) — needs treatment/control data and uplift modeling, not a churn classifier.

Symptom of the trap: a high-accuracy churn or risk model that does not improve the business outcome because the targeted users would have behaved the same way regardless. If the action is the point, frame it as causal/uplift and plan for experimental or quasi-experimental data.

## 3. Problem type

| Output question | Type | Typical eval |
|---|---|---|
| Which class / probability? | classification | AUC, PR, calibration, slice metrics |
| How much / how many? | regression | error metrics, calibration of intervals |
| What order / top-k? | ranking | NDCG, MAP, recall@k |
| What value over future horizon? | forecasting | backtested error by horizon, OOT |
| Effect of an action? | causal/uplift | uplift/Qini, treatment-aware eval |
| Most similar items? | retrieval | recall@k, latency |
| New content? | generation | task-specific + human eval |
| What is unusual? | anomaly | precision@k, review-budget aware |

## 4. Modality

tabular / time-series / text / image / audio / multi-modal. Modality drives feature work, leakage shape, and evaluation:

- time-series/forecasting: horizon-aware backtesting, known-future vs unknown-future split, no shuffled CV.
- text/image/audio: representation choice, leakage via near-duplicates and identity, heavier labeling cost.
- multi-modal: per-source availability at serving time.

This skill's templates are tuned for supervised tabular and time-series risk/ranking work. For retrieval, generation, recommenders, and RL, reuse the framing, data, leakage, evaluation, and serving sections, but expect task-specific evaluation beyond these templates.

## 5. Framing output

Record in the Project Definition Brief, section 2:

- ML needed? + the non-ML baseline.
- Problem type and why.
- Predictive vs causal/uplift.
- Modality and its main implication for data and evaluation.
