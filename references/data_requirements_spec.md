# Data Requirements & Availability Spec

ML differs from ordinary software in one decisive way: the system is mostly defined by data, not by code. So this skill treats the data situation as a first-class planning input, and it must keep working even when the user cannot hand over real data.

Use this artifact right after the Project Definition Brief, before EDA. Its job is to make the user *think less* by giving a default mode and a fixed structure, and to let planning continue gracefully whether data is in hand, only described, restricted, or not yet collected.

## Core principle: absence of data is not a reason to stop planning

The absence of usable data is a reason to *plan the data*, not to stop. When data cannot be inspected, the skill should:

1. proceed with the full plan,
2. convert every claim that cannot be checked into an explicit **assumption**,
3. attach a **validation hook** describing how each assumption will be confirmed once data exists,
4. record any missing-but-required data as a `need_data` boundary item.

Never silently assume data exists, is clean, is large enough, or is shareable.

## Real Data Safety Gate

When the current mode is `data_in_hand` or `data_partial`, and the agent will inspect real files/tables/schemas/samples or run code over user data, apply `references/data_safety.md` before EDA or extraction.

Record the resulting `Data Safety Note` here. If the safety boundary is unclear, do not inspect the data; continue in `data_described` mode on explicit assumptions and validation hooks.

## Data Availability Modes (slide thinking)

Pick exactly one current mode. The mode controls how hard later gates are, and how much of the plan rests on assumptions rather than evidence. Modes can change as data becomes available; re-run the affected EDA/boundary items when they do.

| Mode | Meaning | How the skill behaves |
|---|---|---|
| `data_in_hand` | Real data is accessible now (local files, warehouse, API). | Validate every assumption empirically via EDA before fixing boundaries. |
| `data_partial` | Schema, a small sample, or aggregate stats are available; full data is restricted. | Plan against the known schema/sample; mark the rest as assumptions with validation hooks. |
| `data_described` | User describes the data verbally but cannot share it (private property, NDA, regulated). | Plan entirely on stated assumptions; record each as an assumption to verify; do not require the raw data. |
| `data_absent` | The data does not exist yet and must be collected, logged, purchased, or labeled. | Produce a data acquisition + labeling plan; the plan is a set of hypotheses to be tested after collection. |

Default when the user does not say: ask one short question ("can you share data, describe it, or does it not exist yet?") and otherwise assume `data_described`, because that is the most common private-data case.

## What to capture (works in every mode)

### 1. Required data, derived from the plan (not from what happens to exist)

For each data need implied by the prediction unit, labels, and candidate features:

| Need ID | Purpose (label / feature / evidence / eval) | Entity + time grain | Minimum fields | Source (known or assumed) | History depth needed | Availability mode | Assumption? |
|---|---|---|---|---|---|---|---|

Derive needs top-down from the Project Definition Brief. Do not let the available tables silently define the problem.

### 2. Feasibility check (sufficiency, not just existence)

Answer with real numbers when `data_in_hand`/`data_partial`, and with **stated assumptions + a validation hook** otherwise.

- Approximate number of labeled examples available or obtainable:
- Positive-class prevalence / base rate:
- History depth vs required `as_of_date` lookback:
- Entity/time/source coverage of the prediction population:
- Is the labeled sample plausibly large enough for the chosen problem type? (rough order of magnitude, not a power calculation)
- Cold-start / sparse-history segments:

If sufficiency is unknown and unverifiable now, do not block. Record it as a `need_data` boundary item and a validation hook, and continue planning under a clearly stated assumption.

### 3. Access, privacy, and sharing constraints

- Who can access the raw data, and where can it be processed?
- Privacy/compliance regime (PII, PHI, financial, regional law):
- Can data leave a boundary (no-network / no-export rules)?
- Fields that exist but must never be used (protected attributes, blocked leakage fields):
- Approved inputs, raw-display policy, write/output location, and approval owner:

### 4. Labeling / annotation plan (required when labels do not already exist)

Trigger this section whenever labels are weak, missing, or must be created.

- How are labels produced (logged outcomes, human annotation, weak supervision, purchase)?
- Annotation guideline owner and definition of each class:
- Inter-annotator agreement target and adjudication policy:
- Labeling budget / volume and gold-vs-weak split:
- Active-learning or bootstrapping approach if labels are expensive:

### 5. Assumptions register (the heart of slide thinking)

Every unverified claim the plan depends on goes here, so downstream readers know what is evidence and what is hypothesis.

| Assumption ID | Statement | Why we depend on it | Risk if wrong | Validation hook (EDA/experiment to run when data arrives) | Status |
|---|---|---|---|---|---|

Status values: `assumed`, `validated`, `refuted`, `accepted_risk`.

## Completion Contract

This spec is complete enough to continue when:

- a single current data availability mode is chosen;
- required data is derived from the plan, with grain and minimum fields, not from convenience tables;
- a feasibility judgment exists (real numbers if data is in hand; otherwise explicit assumptions + validation hooks);
- access/privacy constraints and blocked fields are listed;
- if real data will be touched, a `Data Safety Note` records approved inputs, processing boundary, raw-display policy, no-export rule, output location, and approval owner;
- a labeling plan exists whenever labels must be created;
- every unverifiable claim appears in the assumptions register with a validation hook.

Do not treat assumption-based plans as validated. Before implementation, each load-bearing assumption must be `validated` or explicitly `accepted_risk` and recorded as a boundary decision.
