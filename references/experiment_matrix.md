# Experiment Matrix Template

Use this after the baseline dataset and evaluation protocol are defined.

## Fixed Inputs

- Label view:
- Feature view:
- Train/calib/test split:
- `as_of_date` policy:
- Primary metric:
- Required slices:

## Experiments

| Experiment ID | Boundary | Variant A | Variant B | Hold Constant | Metrics | Decision Rule |
|---|---|---|---|---|---|---|
| X-001 | Model family | baseline model | candidate model | labels/features/splits/calibration | primary + slice metrics | candidate wins only if overall and key slices improve |
| X-002 | Calibration | raw score | calibrated score | trained model/features/splits | ECE/Brier/reliability | choose best calibrated option without damaging precision/recall materially |
| X-003 | Sampling | natural training | weighted/sampled training | calib/test natural | model + calibration metrics | use only if it improves target metric and calibration remains acceptable |
| X-004 | Weak/derived evidence | without source | with source | all else fixed | overall + source-only slice | admit only if useful and stable |

## Reporting Requirements

Each experiment should output:

- run manifest
- metrics table
- slice metrics
- calibration metrics when relevant
- feature list
- decision note: accept / reject / continue investigation

## Completion Contract

The experiment matrix is complete only when each experiment has variants, hold-constant fields, metrics, required slices, and a decision rule.

Do not compare models or policies on different labels, splits, feature views, or calibration settings unless the difference is the explicit experiment.
