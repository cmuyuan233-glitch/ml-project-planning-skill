---
name: ml-project-planning
description: Use when starting or reframing a machine-learning or algorithm project before implementation, especially when the user asks to design an ML project, define prediction target/labels/features/evaluation, plan a v2 model with new labels or data sources, create an engineering architecture, assess leakage risks, decide whether modeling is ready, make an algorithm plan, or plan before writing code.
---

# ML Project Planning

## Purpose

Guide early-stage machine-learning projects from a vague business idea to an implementation-ready project definition, boundary register, EDA plan, feature plan, experiment matrix, and engineering architecture.

This skill is for planning before implementation. Its job is to prevent premature modeling before the prediction target, labels, timing, leakage risks, evaluation protocol, and engineering boundaries are clear.

## Core Rule

Do not start coding, model selection, feature implementation, or database extraction until the project definition and key boundaries are explicit.

If the user asks for implementation while project definition is incomplete, first produce or update the planning artifacts and identify what is still missing.

## Hard Gates

These gates are mandatory. Be polite but do not bypass them.

| Missing Item | Must Not Proceed To | Required Response |
|---|---|---|
| Business decision/action | model objective, metrics, delivery policy | Ask who uses the output and what action it changes. |
| Prediction unit | labels, features, datasets, architecture | Ask what one prediction row represents and what keys define it. |
| Prediction timing or `as_of_date` | feature plan, leakage-safe evaluation, scoring plan | Mark future-leakage risk and define the cutoff semantics. |
| Label definition/source | feature admission, model plan, experiment matrix | Define positive/negative/unknown/review-only labels and trust tiers. |
| Error cost/evaluation priority | model comparison, threshold policy | Clarify false-positive/false-negative cost and primary metric. |
| Boundary register | engineering architecture, implementation plan | Create or update fixed/EDA/experiment/deferred boundaries first. |

If a gate blocks progress, give the smallest useful next question or draft the missing section with explicit assumptions for user approval.

## Anti-Slip Rules

Users often ask for implementation before the ML problem is safe to implement. When that happens:

- If asked to choose a model too early, redirect to label, split, and metric definition.
- If asked to build features before prediction timing is clear, stop and define `as_of_date`.
- If asked to use convenient fields that look like labels, audit/review outcomes, future behavior, or downstream scores, flag leakage and classify them as blocked or metadata-only.
- If asked to run broad EDA, tie the EDA back to unresolved boundaries.
- If asked to write code before the architecture is accepted, produce or update the planning artifact instead.
- If the user explicitly accepts a risky shortcut, record it as a boundary decision or known limitation rather than silently adopting it.

## Workflow

### 1. Start With Project Definition

Ask at most three high-leverage questions first:

1. What business decision or action will use the model output?
2. What is one prediction row: user, company, product, company-product pair, event, time window, or something else?
3. What is the label truth, and where does it come from?

Do not ask a long questionnaire up front. After the user answers, draft a `Project Definition Brief` and mark missing items.

Use `references/project_definition_brief.md` when drafting or auditing the brief.

Completion contract: the brief must define business decision, prediction unit, prediction timing, label definition, error cost, evaluation priority, data boundaries, non-goals, and open questions. Use `references/completion_contracts.md` for full checks.

### 2. Separate Data Roles

Classify every data source into one or more roles:

- raw source
- label source
- evidence source
- feature source
- metadata/audit source
- forbidden leakage source
- review-only source

Explicitly flag fields that must not become model features, such as labels, audit flags, future outcomes, human review decisions, downstream model scores, or delivery policy outputs.

Use `references/leakage_checklist.md` when leakage risk is material.

### 3. Build A Boundary Register

Create or update a boundary register with these statuses:

- `fixed_now`: can be decided before EDA/model experiments
- `need_eda`: requires exploratory analysis
- `need_experiment`: requires model or ablation experiments
- `deferred`: important but intentionally postponed
- `rejected`: considered and rejected

Do not force uncertain decisions. Turn uncertainty into an EDA or experiment item.

Use `references/boundary_decision_register.md` as the template.

Completion contract: every unresolved boundary must be classified as `need_eda`, `need_experiment`, `deferred`, or `rejected`; do not leave ambiguous open issues in prose only.

### 4. Define EDA From Boundaries

EDA should answer boundary questions, not wander through data aimlessly.

For each `need_eda` boundary, define:

- required local/source data
- grain of analysis
- metrics or summaries
- expected output artifact
- decision that the EDA should enable

Use `references/eda_plan.md` as the template.

Completion contract: every EDA item must name the boundary it answers, input data, grain, metrics, output artifact, and decision it enables.

### 5. Plan Features With Admission Classes

Every proposed feature should have:

- name
- class: `baseline_model_feature`, `experiment_feature`, `metadata_only`, or `blocked`
- why it exists
- how it is built
- leakage notes
- required source fields

Composite scores, manual priors, and rule-like fields should usually start as `experiment_feature`, not baseline.

Use `references/feature_construction_plan.md` as the template.

Completion contract: every feature row must include class, why, how, source fields, and leakage notes.

### 6. Define Experiments After The Baseline

Model and sampling decisions belong in an experiment matrix after the label, feature, split, and evaluation protocol are stable.

Common experiment boundaries:

- model family comparison
- calibration method
- class imbalance handling
- manual prior scores
- composite features
- weak/LLM-derived evidence inclusion
- delivery guardrails

Use `references/experiment_matrix.md` as the template.

Completion contract: every experiment must define variants, what stays constant, metrics, slices, and a decision rule.

### 7. Produce The Engineering Architecture

Only after the definition and boundaries are clear, draft an engineering architecture that includes:

- repository/package layout
- data source and artifact policy
- label store
- evidence layer
- feature table
- dataset builder
- model training
- calibration/evaluation
- scoring/delivery separation
- manifests/versioning
- no-go environment rules
- non-blocking validation work

Use `references/engineering_architecture.md` as the template.

Completion contract: the architecture must include layer contracts, repository/artifact layout, manifests, leakage controls, time policy, environment rules, and non-blocking validation work.

## Output Sequence

Prefer this artifact order:

1. `Project Definition Brief`
2. `Boundary Decision Register`
3. `EDA Plan`
4. `Feature Construction Plan`
5. `Experiment Matrix`
6. `Engineering Architecture`
7. Implementation plan only after the above are accepted

For small projects, these can be concise sections in one document. For larger projects, split them into separate markdown files.

## Quality Gate Before Implementation

Before recommending implementation, verify:

- The business decision is explicit.
- The prediction unit is explicit.
- The prediction timing or `as_of_date` semantics are explicit.
- Positive, negative, unknown, weak, and review-only labels are separated.
- Leakage risks are listed and blocked by policy.
- Calibration/test distribution policy is clear.
- EDA and experiment-only decisions are not hard-coded as engineering facts.
- Output artifacts and manifests are specified.
- Non-goals and environment restrictions are recorded.

Use `references/completion_contracts.md` for gate checks and `references/quality_rubric.md` for scoring outputs.

## Testing This Skill

When improving this skill, test it on realistic and adversarial project prompts. Use `references/test_cases.md` and score the outputs with `references/quality_rubric.md`.

The best success criterion is not a polished document. It is whether the skill stops the project from entering modeling before labels, timing, leakage, and evaluation are clear.
