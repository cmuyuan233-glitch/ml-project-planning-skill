# ML Engineering Architecture Template

Use after the project definition and boundary register are accepted.

## Purpose

- Business objective:
- ML objective:
- Main prediction unit:
- Main output:

## Hard Boundaries

| Boundary | Decision |
|---|---|
| Code/artifact isolation |  |
| Data access rules |  |
| Label leakage controls |  |
| Time leakage controls |  |
| Model score vs business decision |  |
| Manifest/versioning |  |

## Repository Layout

```text
<project>/
  config/
  common/
  stages/
  tests/
  artifacts/
```

## Pipeline

```text
source snapshots
  -> label store
  -> evidence/events/features
  -> feature table
  -> datasets
  -> training
  -> calibration
  -> evaluation
  -> scoring
  -> delivery policy
  -> monitoring + feedback loop
```

## Layer Contracts

### Label Store

- grain:
- labels:
- metadata-only fields:
- blocked fields:

### Evidence/Event Layer

- grain:
- source roles:
- event time policy:
- confidence/source metadata:

### Feature Table

- grain:
- baseline feature groups:
- experiment feature groups:
- allowlist/blocklist:

### Dataset Builder

- split policy:
- sampling policy:
- calibration/test policy:
- time/OOT policy:

### Model Layer

- baseline model:
- candidate models:
- model registry:

### Calibration/Evaluation

- calibration candidates:
- primary metrics:
- required slices:

### Delivery

- model output:
- policy output:
- review/audit outputs:

### Serving & Training-Serving Parity

The most common production failure is that features computed at serving time differ from those computed in training.

- serving mode: batch / online / streaming
- where features come from at inference time (must match training `as_of_date` semantics):
- features unavailable or delayed at serving time (must be excluded or imputed identically):
- parity check: same code/contract builds train and serve features? yes/no

### Monitoring & Feedback Loop

A model is not done at deployment; it decays. Plan this before launch.

- input drift / data-quality monitors:
- prediction distribution and score-stability monitors:
- performance monitoring once labels mature (and label latency):
- fairness/slice monitoring over time:
- retraining trigger: scheduled / drift-based / performance-based
- rollout strategy: shadow / canary / A-B / full
- feedback-loop risk: do today's predictions shape tomorrow's labels or features? how is it controlled?

## Manifest Policy

Every stage should record:

- input files and hashes
- output files and hashes
- row counts
- schema version
- config hash
- code version
- known limitations
- warnings

## Non-Blocking Validation Work

| Item | Validation Needed | Blocks |
|---|---|---|

## Completion Contract

The architecture is complete enough for implementation planning only when it defines repository layout, data access rules, layer contracts, artifact/manifest policy, leakage controls, time policy, serving and training-serving parity, monitoring and feedback-loop policy, non-goals, and non-blocking validation work.

Do not move to implementation if model outputs and business decisions are conflated, environment restrictions are missing, artifact ownership is ambiguous, training-serving parity is undefined, or there is no monitoring/retraining plan for a model that will run in production.
