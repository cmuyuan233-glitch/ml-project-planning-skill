# Boundary Decision Register Template

Use this after the Project Definition Brief. The goal is to separate decisions that can be fixed now from decisions that require data or model evidence.

## Status Legend

| Status | Meaning |
|---|---|
| `fixed_now` | Can be decided before EDA or model experiments. |
| `need_data` | Cannot be decided until data is collected, shared, or labeled. Plan continues on a stated assumption. |
| `need_eda` | Requires exploratory data analysis before fixing. |
| `need_experiment` | Requires model or ablation experiments before fixing. |
| `deferred` | Important but intentionally postponed. |
| `rejected` | Considered and rejected. |

When data is absent, described-only, or restricted, do not force a `fixed_now` or `need_eda` decision. Use `need_data` and pair it with an assumption + validation hook in the Data Requirements Spec.

## Fixed Engineering Principles

| ID | Boundary Question | Decision | Status | Affects |
|---|---|---|---|---|
| B-001 | What is the prediction unit? |  | `fixed_now` | labels, features, evaluation |
| B-002 | What is the prediction cutoff/as_of_date policy? |  | `fixed_now` | leakage, features, scoring |
| B-003 | What data or operations are forbidden? |  | `fixed_now` | environment, implementation |
| B-004 | Which fields are blocked from model features? |  | `fixed_now` | leakage, feature store |
| B-005 | Are model score and business decision separated? |  | `fixed_now` | scoring, delivery |
| B-006 | Are artifacts/manifests required? |  | `fixed_now` | reproducibility |

## Boundaries Requiring EDA

| ID | Boundary Question | Current Default | Required EDA | Affects |
|---|---|---|---|---|
| E-001 | Which data sources have usable coverage? |  | coverage by label/time/entity | features |
| E-002 | Which labels are trustworthy? |  | conflict and age analysis | label policy |
| E-003 | Which features are missing, unstable, or risky? |  | missingness, drift, leakage audit | feature policy |

## Boundaries Requiring Data

Use when a decision is blocked only by data access, volume, or labeling. The plan proceeds on the stated assumption; the validation hook is run once data exists.

| ID | Boundary Question | Working Assumption | Data Needed To Confirm | Validation Hook | Affects |
|---|---|---|---|---|---|
| D-001 | Are there enough labeled positives to model this? |  | label volume + base rate | label EDA when data arrives | feasibility, scope |
| D-002 | Do candidate features actually exist at `as_of_date`? |  | feature coverage at cutoff | time/coverage EDA | feature plan |
| D-003 | Is label truth trustworthy and consistent? |  | conflict/agreement stats | label trust EDA | label policy |

## Boundaries Requiring Experiments

| ID | Boundary Question | Current Default | Required Experiment | Affects |
|---|---|---|---|---|
| X-001 | Which model family should be main? | first simple baseline | same split/features/calibration comparison | model registry |
| X-002 | Which calibration method should be used? | compare common options | reliability/ECE/Brier | calibration |
| X-003 | Is resampling or weighting needed? | keep calib/test natural | natural vs weights vs train-only sampling | training |
| X-004 | Should weak/LLM-derived features enter main model? | experiment only | with/without ablation and slice metrics | feature policy |

## Decision Log

| Date | Boundary ID | Evidence / Artifact | Decision Change | Notes |
|---|---|---|---|---|

## Completion Contract

This register is complete enough for architecture work only when every important unresolved question is assigned to `fixed_now`, `need_data`, `need_eda`, `need_experiment`, `deferred`, or `rejected`.

Do not leave key decisions as informal prose. If a decision is blocked by data access/volume/labeling, create a `need_data` row with an assumption and validation hook. If it needs analysis of data already in hand, create a `need_eda` row. If it needs model evidence, create a `need_experiment` row.
