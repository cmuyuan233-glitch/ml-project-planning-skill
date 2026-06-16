# Glossary

Short definitions for terms used across this skill, so the planning artifacts stay usable for less experienced data scientists.

- **`as_of_date` / cutoff**: the moment a prediction is made. Only information available strictly before it may become a feature. Anything after it is future leakage.
- **Prediction unit**: what one row of the dataset represents, defined by its entity and time keys (e.g. `user_id + as_of_date`).
- **Leakage**: information in the features that would not be available at real prediction time, or that encodes the label. Inflates offline metrics and collapses in production.
- **Label trust tiers**: separating gold (trusted) labels from weak/heuristic labels and from review-only signals, so weak labels are not treated as ground truth.
- **Base rate / prevalence**: the fraction of positives in the population. Drives metric choice and how many labels you need.
- **Calibration**: whether predicted probabilities match observed frequencies (a 0.7 score should be right ~70% of the time). Measured by reliability curves, **ECE** (expected calibration error), and **Brier** score.
- **OOT (out-of-time) evaluation**: testing on a later time period than training, to detect time leakage and drift. Required for time-dependent problems.
- **Slice metrics**: performance measured per subgroup (tier, region, tenure, sensitive group), not just overall.
- **Precision@k**: of the top k flagged items (often set by review/outreach capacity), the fraction that are true positives.
- **Drift**: change over time in inputs (data drift) or in the input-output relationship (concept drift); a key reason models decay.
- **Training-serving skew**: features computed differently at training vs serving time, the most common production ML bug.
- **Uplift / causal modeling**: estimating the *effect of an action* (who changes behavior if treated), as opposed to predicting an outcome under no action. Needs treatment/control data.
- **Heuristic baseline**: a simple non-ML rule that any model must beat to justify its cost.
- **Data availability mode**: whether data is in hand, partial, described-only, or absent — controls how much of the plan rests on assumptions vs evidence.
- **Validation hook**: a specific EDA/experiment to run once data is available, to confirm or refute an assumption the plan depends on.
