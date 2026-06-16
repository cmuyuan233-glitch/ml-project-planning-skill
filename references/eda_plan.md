# Boundary-Driven EDA Plan Template

EDA should answer boundary questions. Avoid broad wandering analysis unless needed for data quality.

When data is not yet available (`data_described` or `data_absent`), do not skip this step. Instead, write each EDA item as a **validation hook**: the exact analysis to run on the first real data pull to confirm or refute an assumption from `references/data_requirements_spec.md`. Each `need_data` boundary should map to a hook here.

## EDA Inventory

| EDA ID | Boundary | Question | Input Data | Grain | Output Artifact | Decision Enabled |
|---|---|---|---|---|---|---|

## Standard Checks

### Label EDA

- label counts
- positive/negative/unknown distribution
- label source distribution
- label age/time distribution
- duplicate and conflict rates
- gold vs weak disagreement

### Coverage EDA

- entity coverage
- pair/event coverage
- source/channel coverage
- product/category coverage if relevant
- missing key fields

### Time EDA

- source record time availability
- snapshot fallback rate
- label time distribution
- feature availability before/after cutoff
- time split/OOT feasibility

### Leakage EDA

- fields with label-like names
- audit/review/model-score columns
- future outcome columns
- post-decision fields
- source timestamps after label/scoring cutoff

### Feature EDA

- missing rate
- low variance
- univariate relationship with labels
- distribution by label source and time
- drift or instability
- high-cardinality categorical behavior

## EDA Output Requirements

Each EDA should produce:

- a machine-readable table under the project EDA output directory;
- a short markdown interpretation;
- a recommendation to fix, defer, or experiment with the boundary.

## Completion Contract

The EDA plan is complete only when every EDA item maps to a boundary, names input data, defines grain, specifies metrics/summaries, declares output artifacts, and states what decision the EDA should enable.

Do not run broad EDA that is not tied to a boundary or data quality risk.
