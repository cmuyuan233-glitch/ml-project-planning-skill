---
name: ml-project-planning
description: Use when starting or reframing a machine-learning or algorithm project before implementation, especially when the user asks to design an ML project, decide whether ML is even needed, frame a problem (predictive vs causal/uplift), define prediction target/labels/features/evaluation, plan a v2 model with new labels or data sources, plan when data is private/undisclosed or not yet collected, set success thresholds and fairness/compliance needs, create an engineering and serving/monitoring architecture, assess leakage risks, decide whether modeling is ready, make an algorithm plan, or plan before writing code.
---

# ML Project Planning

## Purpose

Turn a vague ML idea into an implementation-ready plan: project definition, problem framing, data requirements, boundary register, EDA plan, feature plan, experiment matrix, and an engineering + serving/monitoring architecture.

The skill exists to let the data scientist or ML engineer **think less and decide better**: it supplies the structure, the default choices, and the questions that matter, so good planning becomes filling in a well-shaped template instead of inventing process from scratch.

## Design Principles

1. **Defaults over blank pages.** When the user is unsure, offer the sensible default and mark it as an assumption. Do not stall on a blank.
2. **Data is the substrate.** ML projects are defined by data, not just code. Treat the data situation as a first-class input, not an afterthought.
3. **Degrade gracefully without data (slide thinking).** Data is often private, restricted, or not yet collected. Absence of data is a reason to *plan the data*, not to stop. Continue on explicit assumptions with validation hooks; never silently assume data exists.
4. **Leakage and timing first.** Most ML failures come from leakage and undefined prediction timing, so gate on these hardest.
5. **Change one thing at a time.** Comparisons and decisions isolate a single factor.

## Core Rule

Planning may always proceed, even with no data. What must not happen is treating an assumption-based plan as validated, or starting implementation, model selection, feature building, or data extraction before the project definition and key boundaries are explicit.

If the user asks for implementation while the definition is incomplete, produce or update the planning artifacts and identify what is still missing.

## How To Use This Skill

Pick the lightest track that fits.

- **Fast lane** (small, low-stakes, or exploratory work): produce a one-page Project Definition Brief, a short data-availability + feasibility note, and a leakage check, then a go/no-go. Skip the heavier artifacts unless a gate fails.
- **Full track** (production, high-stakes, or people-affecting work): produce the full artifact sequence below.

A senior user may deliberately relax a gate; record the relaxation as an accepted-risk boundary decision rather than skipping it silently.

## Hard Gates

These gates are mandatory. Be polite but do not bypass them. In `data_described`/`data_absent` mode, a gate is satisfied by an explicit assumption plus a validation hook, not by inspecting real data.

| Missing Item | Must Not Proceed To | Required Response |
|---|---|---|
| Business decision/action | model objective, metrics, delivery policy | Ask who uses the output and what action it changes. |
| Problem frame (ML-vs-heuristic, predictive-vs-causal) | model plan | Confirm ML is needed and whether the real question is predictive or causal/uplift. |
| Prediction unit | labels, features, datasets, architecture | Ask what one prediction row represents and what keys define it. |
| Prediction timing or `as_of_date` | feature plan, leakage-safe evaluation, scoring plan | Mark future-leakage risk and define the cutoff semantics. |
| Label definition/source | feature admission, model plan, experiment matrix | Define positive/negative/unknown/review-only labels and trust tiers. |
| Error cost + success threshold | model comparison, threshold policy | Clarify FP/FN cost and the minimum performance worth shipping. |
| Data availability mode | feasibility claims, EDA, scoring plan | State whether data is in hand, described, or absent; continue on assumptions if needed. |
| Real-data safety boundary, when data will be inspected or processed | EDA, data extraction, profiling, dataset export, external tool use | Apply the data safety gate: approved inputs, processing location, no-export rule, raw-display policy, sensitive fields, and output path. |
| Boundary register | engineering architecture, implementation plan | Create or update fixed/data/EDA/experiment/deferred boundaries first. |

If a gate blocks progress, give the smallest useful next question or draft the missing section with explicit assumptions for user approval.

## Anti-Slip Rules

Users often ask for implementation before the ML problem is safe to implement. When that happens:

- If asked to choose a model too early, redirect to label, split, and metric definition.
- If asked to build features before prediction timing is clear, stop and define `as_of_date`.
- If asked to use convenient fields that look like labels, audit/review outcomes, future behavior, or downstream scores, flag leakage and classify them as blocked or metadata-only.
- If the user has no data or cannot share it, do not stop: set the data availability mode, plan on stated assumptions, and attach validation hooks. Never silently assume data exists, is clean, is large enough, or is shareable.
- If asked to inspect files, query tables, run EDA/code, or export artifacts over real user data before a safety boundary is explicit, stop and apply the data safety gate first.
- If the action triggered by the model is the real goal, check for the causal/uplift trap instead of optimizing prediction accuracy alone.
- If no non-ML baseline exists, define one before comparing models.
- If asked to run broad EDA, tie the EDA back to unresolved boundaries (or write it as a validation hook when data is absent).
- If asked to write code before the architecture is accepted, produce or update the planning artifact instead.
- If the user explicitly accepts a risky shortcut, record it as a boundary decision or known limitation rather than silently adopting it.

## Workflow

### 1. Frame The Problem And Data Situation

Before defining details, settle the shape of the work:

- Is ML needed, or is a heuristic enough? Name the non-ML baseline either way.
- Problem type and modality, and whether the real question is predictive or causal/uplift.
- Data availability mode: `data_in_hand`, `data_partial`, `data_described`, or `data_absent`. If unstated, ask one short question and otherwise assume `data_described`.
- If real data, samples, schemas, logs, or user-data code execution are involved, confirm the data safety boundary before reading or processing them.

Use `references/problem_framing.md`, `references/data_requirements_spec.md`, and `references/data_safety.md` when real data may be touched.

### 2. Start With Project Definition

Ask at most three high-leverage questions first:

1. What business decision or action will use the model output?
2. What is one prediction row: user, company, product, event, time window, or something else?
3. What is the label truth, and where does it come from?

Do not ask a long questionnaire up front. After the user answers, draft a `Project Definition Brief` and mark missing items as assumptions or open questions.

Use `references/project_definition_brief.md`. Completion checks are in `references/completion_contracts.md`.

### 3. Specify Data Requirements And Feasibility

Derive required data top-down from the definition (not from whatever tables happen to exist). Record the availability mode, a feasibility judgment (real numbers if data is in hand; otherwise assumptions + validation hooks), access/privacy constraints, the data safety note when real data will be touched, a labeling plan if labels must be created, and an assumptions register.

Use `references/data_requirements_spec.md` and `references/data_safety.md` when real data may be touched.

### 4. Separate Data Roles

Classify every data source into one or more roles: raw, label, evidence, feature, metadata/audit, forbidden-leakage, review-only. Explicitly flag fields that must not become model features (labels, audit flags, future outcomes, human review decisions, downstream model scores, delivery outputs).

Use `references/leakage_checklist.md` when leakage risk is material.

### 5. Build A Boundary Register

Create or update a boundary register with statuses `fixed_now`, `need_data`, `need_eda`, `need_experiment`, `deferred`, `rejected`. Turn uncertainty into a typed work item rather than forcing a decision. Use `need_data` when a decision is blocked only by data access, volume, or labeling, and pair it with an assumption and validation hook.

Use `references/boundary_decision_register.md`.

### 6. Define EDA From Boundaries

For each `need_eda` boundary define: required data, grain, metrics/summaries, expected output artifact, and the decision it enables. When data is absent, write each item as a validation hook to run on the first real data pull.

Use `references/eda_plan.md`.

### 7. Plan Features With Admission Classes

Every feature gets: name, class (`baseline_model_feature`, `experiment_feature`, `metadata_only`, `blocked`), why, how it is built, leakage notes, source fields, and a serving-time availability check (training-serving parity). Composite scores, manual priors, and rule-like fields usually start as `experiment_feature`.

Use `references/feature_construction_plan.md`.

### 8. Define Experiments After The Baseline

Model and sampling decisions belong in an experiment matrix after label, feature, split, and evaluation protocol are stable. Always include the heuristic-vs-ML baseline experiment; include an uplift experiment when the framing is causal.

Use `references/experiment_matrix.md`.

### 9. Address Responsible-ML Obligations

Identify sensitive attributes and proxies, fairness slices, explainability/compliance constraints, and the project's value bar. For low-stakes internal models, an explicit "low stakes, N/A because ..." note is acceptable.

Use `references/responsible_ml.md`.

### 10. Produce The Engineering & Serving Architecture

Only after definition and boundaries are clear, draft an architecture covering repository/artifact layout, data access policy, label/evidence/feature/dataset layer contracts, training, calibration/evaluation, scoring/delivery separation, **serving mode and training-serving parity**, **monitoring, drift, retraining, and feedback-loop policy**, manifests/versioning, environment rules, and non-blocking validation work.

Use `references/engineering_architecture.md`.

## Output Sequence

Prefer this order (collapse to the Fast Lane subset for small projects):

1. `Project Definition Brief`
2. `Data Requirements & Availability Spec`
3. `Boundary Decision Register`
4. `EDA Plan`
5. `Feature Construction Plan`
6. `Experiment Matrix`
7. `Engineering & Serving Architecture`
8. Implementation plan only after the above are accepted

For small projects these can be concise sections in one document; for larger projects, split into separate files.

## Quality Gate Before Implementation

Before recommending implementation, verify:

- The business decision and problem frame (incl. predictive vs causal) are explicit.
- The prediction unit and prediction timing / `as_of_date` semantics are explicit.
- Positive, negative, unknown, weak, and review-only labels are separated.
- The data availability mode is stated and load-bearing assumptions have validation hooks; nothing about data is silently assumed.
- Real data access has a safety boundary when applicable: approved inputs, processing location, no-export/no-network rules, raw-row display policy, sensitive fields, read-only source policy, and output location.
- A success threshold / kill-go bar exists, and a non-ML heuristic baseline plus the `X-000` (heuristic-vs-ML) experiment are defined. Whether ML actually beats the baseline is decided by that experiment, not asserted here.
- Leakage risks are listed and blocked by policy.
- Calibration/test distribution policy is clear.
- Fairness/compliance obligations are addressed (or explicit low-stakes N/A).
- Training-serving parity and a monitoring/retraining plan exist for production models.
- EDA and experiment-only decisions are not hard-coded as engineering facts.
- Output artifacts and manifests are specified; non-goals and environment restrictions are recorded.

## Adoption Gate Before Deployment

These are checked after experiments run, not before implementation:

- The `X-000` experiment shows ML clears the success threshold and beats the non-ML baseline by enough to justify its cost; otherwise ship the heuristic or rescope.
- Calibration, slice, and (if applicable) fairness metrics meet their thresholds.
- For causal framing, uplift improves the acted-on outcome (not just prediction accuracy).

Use `references/completion_contracts.md` (canonical gates) and `references/quality_rubric.md` for scoring. New terms are defined in `references/glossary.md`, and `references/worked_example.md` shows a full filled-in plan for a private-data case.

## Testing This Skill

When improving this skill, test it on realistic and adversarial prompts, including private/no-data and causal-trap cases. Use `references/test_cases.md` and score with `references/quality_rubric.md`.

The best success criterion is not a polished document. It is whether the skill stops the project from entering modeling before framing, data reality, labels, timing, leakage, and evaluation are clear — while still moving the plan forward when data cannot yet be seen.
