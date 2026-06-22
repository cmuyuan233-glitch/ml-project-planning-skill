# ML Project Planning Skill

A planning skill that turns a vague machine-learning idea into an implementation-ready plan, so a data scientist or ML engineer can **think less and decide better**. It supplies the structure, the default choices, and the questions that matter most, and it deliberately keeps working when the data is private, described-only, or not yet collected.

Its core job is to stop a project from sliding into modeling before the problem frame, data reality, labels, prediction timing, leakage risks, and evaluation protocol are clear.

## When this skill applies

Use it when starting or reframing an ML/algorithm project before implementation — for example:

- designing an ML project from a business idea, or deciding whether ML is even needed;
- framing the problem (predictive vs causal/uplift, problem type, modality);
- defining the prediction target, labels, features, and evaluation;
- planning when data is private/undisclosed or does not exist yet;
- setting success thresholds and fairness/compliance needs;
- assessing leakage risks and prediction-timing (`as_of_date`) safety;
- planning a v2 model with new labels or data sources;
- producing an engineering + serving/monitoring architecture before writing code.

## Design principles

1. **Defaults over blank pages.** When unsure, take the sensible default and mark it as an assumption instead of stalling.
2. **Data is the substrate.** ML projects are defined by data, not just code, so the data situation is a first-class input.
3. **Degrade gracefully without data (slide thinking).** Data is often private or uncollected. Absence of data is a reason to *plan the data*, not to stop — continue on explicit assumptions with validation hooks, and never silently assume data exists.
4. **Leakage and timing first.** Most ML failures come from leakage and undefined prediction timing, so the hardest gates sit there.
5. **Change one thing at a time.** Comparisons and decisions isolate a single factor.

## How to use it

Pick the lightest track that fits the work:

- **Fast lane** (small, low-stakes, or exploratory): a one-page Project Definition Brief, a short data-availability + feasibility note, and a leakage check, then a go/no-go.
- **Full track** (production, high-stakes, or people-affecting): the full artifact sequence below.

### Data availability modes

The skill picks one mode and adapts how much of the plan rests on assumptions vs evidence:

| Mode | Meaning |
|---|---|
| `data_in_hand` | Real data is accessible now; validate assumptions empirically. |
| `data_partial` | Schema/sample/aggregates known, full data restricted. |
| `data_described` | Data described verbally but not shareable (private/regulated). |
| `data_absent` | Data does not exist yet and must be collected or labeled. |

When data cannot be inspected, every unverifiable claim becomes an explicit assumption with a **validation hook** (the EDA/experiment to run once data exists), and data-blocked decisions are tracked as `need_data` boundaries.

When real data, samples, schemas, logs, or user-data code execution are involved, the skill first applies a data safety gate so approved inputs, processing boundaries, raw-display rules, external-transfer limits, and output locations are explicit.

### Artifact sequence

1. Project Definition Brief
2. Data Requirements & Availability Spec
3. Boundary Decision Register
4. EDA Plan
5. Feature Construction Plan
6. Experiment Matrix
7. Engineering & Serving Architecture
8. Implementation plan (only after the above are accepted)

Planning may proceed before deployment; the actual "ML beats the baseline" decision happens at the adoption gate, after the `X-000` experiment runs.

## Repository layout

```text
.
├── SKILL.md                # the skill: principles, gates, workflow, output sequence
├── agents/
│   └── openai.yaml         # interface metadata (display name, default prompt)
└── references/             # templates and checklists used by the workflow
```

### Reference files

| File | Purpose |
|---|---|
| `references/project_definition_brief.md` | First artifact: decision, framing, prediction unit/timing, labels, error cost, success threshold, data, fairness. |
| `references/problem_framing.md` | ML-vs-heuristic, problem type/modality, and the predictive-vs-causal/uplift trap. |
| `references/data_requirements_spec.md` | Data availability modes, feasibility, labeling plan, and the assumptions register (slide thinking). |
| `references/data_safety.md` | Safety gate before inspecting or processing real user data. |
| `references/boundary_decision_register.md` | Typed status for every open decision: `fixed_now` / `need_data` / `need_eda` / `need_experiment` / `deferred` / `rejected`. |
| `references/leakage_checklist.md` | Label / future / source / model / operational leakage controls. |
| `references/eda_plan.md` | Boundary-driven EDA; becomes validation hooks when data is absent. |
| `references/feature_construction_plan.md` | Feature admission classes, leakage notes, and serving-time parity. |
| `references/experiment_matrix.md` | Controlled experiments incl. heuristic-vs-ML (`X-000`) and uplift (`X-005`). |
| `references/responsible_ml.md` | Fairness slices, proxies, compliance/explainability, and the value bar. |
| `references/engineering_architecture.md` | Layer contracts, serving parity, monitoring/drift/retraining, manifests. |
| `references/completion_contracts.md` | Canonical stage gates between planning steps. |
| `references/quality_rubric.md` | Scoring rubric and automatic gate failures. |
| `references/glossary.md` | Plain-language definitions of the terms used throughout. |
| `references/worked_example.md` | A fully filled-in plan for a private-data churn case. |
| `references/test_cases.md` | Realistic and adversarial prompts for testing the skill. |

## Testing the skill

Run it against the prompts in `references/test_cases.md` (including the private/no-data and causal-trap cases) and score the outputs with `references/quality_rubric.md`. The success criterion is not a polished document — it is whether the skill prevents premature modeling while still moving the plan forward when data cannot yet be seen.
