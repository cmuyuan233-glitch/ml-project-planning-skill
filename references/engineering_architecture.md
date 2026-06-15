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

The architecture is complete enough for implementation planning only when it defines repository layout, data access rules, layer contracts, artifact/manifest policy, leakage controls, time policy, non-goals, and non-blocking validation work.

Do not move to implementation if model outputs and business decisions are conflated, environment restrictions are missing, or artifact ownership is ambiguous.
