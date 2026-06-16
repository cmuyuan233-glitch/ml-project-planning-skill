# Feature Construction Plan Template

Every feature should be intentionally admitted. Do not mix metadata, labels, audit fields, and model inputs.

## Feature Grain

```text
<entity keys> + <time/as_of keys>
```

## Feature Admission Classes

| Class | Meaning |
|---|---|
| `baseline_model_feature` | Included in the first baseline model. |
| `experiment_feature` | Built for ablation but not automatically included. |
| `metadata_only` | Kept for slicing/audit/delivery, forbidden as model input. |
| `blocked` | Must not enter model features. |

## Feature Table

| Feature | Class | Why | How To Build | Source Fields | Leakage Notes |
|---|---|---|---|---|---|

## Recommended Feature Groups

Use only groups that fit the project.

### Identity Metadata

Keys, names, timestamps, source ids. Usually `metadata_only`.

### Label Metadata

Label source, trust tier, conflict flags, review flags. Usually `blocked` for model input.

### Evidence Counts

Counts, distinct source counts, flags, ratios.

### Recency

Latest event age, first event age, event count by time bucket. Requires `as_of_date`.

### Action Or Behavior

Observed actions, event types, transaction roles, page behaviors, status transitions.

### Entity Profile

Stable attributes available before the cutoff.

### Text Or LLM-Derived Features

Parser output, confidence, target type, extraction quality. Start as baseline only after parser quality is audited; otherwise experiment.

### Composite Scores

Domain scores, priors, rule-combinations. Usually `experiment_feature`.

## Required Guards

- Apply allowlist/blocklist before dataset export.
- Keep feature schema versioned.
- Emit missingness and drift summaries.
- Emit feature source manifest.
- Keep derived features reproducible from source artifacts.
- Confirm each baseline feature is actually available at serving time with the same `as_of_date` semantics as training (training-serving parity). If a feature is unavailable or delayed at inference, drop it from baseline or define identical imputation for both train and serve.

When data is not yet in hand, mark feature existence and serving availability as assumptions with validation hooks in `references/data_requirements_spec.md`.

## Completion Contract

The feature plan is complete only when every feature has class, why, how to build, source fields, and leakage notes.

Do not admit experiment features into the baseline without an explicit experiment decision. Do not admit metadata-only or blocked fields into model input.
