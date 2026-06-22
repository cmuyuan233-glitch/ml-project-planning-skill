# Completion Contracts

This file is the canonical source of the stage gates. The gate table in `SKILL.md` and the scoring rubric in `references/quality_rubric.md` summarize these; if they ever disagree, this file wins.

Use these contracts before advancing from one planning stage to the next. If a contract fails, stop and ask the smallest question that unlocks the next step, or draft the missing section with explicit assumptions for user approval.

Data-availability note: a contract that requires evidence (counts, distributions, coverage) is satisfied in `data_described`/`data_absent` mode by a stated assumption plus a validation hook. The requirement is that the item is explicit, not that real data was inspected. Never silently assume; record it.

## Project Definition Brief

Required:

- business decision/action and user of the output;
- problem framing: ML-vs-heuristic, problem type, predictive-vs-causal, modality;
- prediction unit and entity/time keys;
- prediction timing or `as_of_date` semantics;
- model output contract;
- positive, negative, unknown, weak, and review-only label policy;
- label source of truth and trust tiers;
- false-positive and false-negative cost;
- success threshold / kill-go bar and rough expected value;
- primary evaluation priority;
- data availability mode and a feasibility judgment;
- fairness/compliance obligations (or an explicit low-stakes N/A note);
- forbidden sources/fields/operations;
- non-goals;
- open questions.

May advance when:

- prediction unit, label definition, and prediction timing are explicit;
- missing details are listed as open questions rather than hidden assumptions.

Must not advance when:

- "good/bad", "risk", "churn", "operate", or similar target words are undefined;
- one prediction row is ambiguous;
- label truth is unknown;
- prediction time is missing;
- the data availability mode is unstated (default to `data_described` and proceed on assumptions rather than blocking).

## Data Requirements Spec

Required:

- one current data availability mode (`data_in_hand` / `data_partial` / `data_described` / `data_absent`);
- required data derived from the plan, with grain and minimum fields;
- a feasibility judgment (real numbers if data is in hand; otherwise explicit assumptions + validation hooks);
- access/privacy constraints and blocked fields;
- a data safety note when real data, schemas, samples, exports, or user-data execution will be touched;
- a labeling plan when labels must be created;
- an assumptions register entry for every unverifiable load-bearing claim.

May advance when:

- the plan can proceed on stated assumptions even if no data was inspected;
- each `need_data` boundary has an assumption and a validation hook.

Must not advance when:

- the plan silently assumes data exists, is clean, is large enough, or is shareable;
- real data is inspected, profiled, exported, or sent to external tools before the approved inputs, processing boundary, raw-display policy, no-export rule, output location, and approval owner are explicit;
- load-bearing assumptions are missing from the register.

## Boundary Decision Register

Required:

- fixed engineering boundaries;
- data-blocked boundaries (`need_data`) when data is restricted, absent, or unlabeled;
- EDA-required boundaries;
- experiment-required boundaries;
- deferred or rejected items when relevant;
- decision log for user-confirmed changes.

May advance when:

- every important uncertainty has a status (`fixed_now`, `need_data`, `need_eda`, `need_experiment`, `deferred`, or `rejected`);
- each `need_data` item names a working assumption and a validation hook;
- each `need_eda` item names the evidence needed;
- each `need_experiment` item names the comparison needed.

Must not advance when:

- unresolved decisions remain as prose only;
- a data-blocked decision is left without a working assumption and validation hook;
- experiment choices such as model family, calibration, or sampling are hard-coded without evidence.

## EDA Plan

Required for each EDA item:

- boundary id;
- question;
- input data;
- analysis grain;
- metrics or summaries;
- output artifact;
- decision enabled.

May advance when:

- EDA work is tied to unresolved boundaries;
- expected outputs are inspectable tables/reports.

Must not advance when:

- EDA is a generic "explore the data" request with no decision target.

## Feature Construction Plan

Required for each feature:

- feature name;
- admission class: `baseline_model_feature`, `experiment_feature`, `metadata_only`, or `blocked`;
- why it is useful;
- how to build it;
- source fields;
- leakage notes;
- serving availability / training-serving parity (real, or an assumption + validation hook).

May advance when:

- blocked and metadata-only fields are separated from model inputs;
- experiment features are not silently included in baseline;
- feature grain is explicit.

Must not advance when:

- label source, audit outcomes, review flags, future behavior, or model scores appear as model features;
- feature grain is ambiguous;
- `as_of_date` or cutoff behavior is missing for time-dependent features.

## Experiment Matrix

Required for each experiment:

- boundary id;
- variants;
- what stays constant;
- metrics;
- required slices;
- decision rule.

May advance when:

- baseline inputs are fixed;
- comparisons change one meaningful factor at a time;
- calibration/test distribution policy is clear.

Must not advance when:

- model comparisons use different labels, features, or splits without saying so;
- threshold or delivery policy is optimized before calibration/evaluation is understood.

## Engineering Architecture

Required:

- repository/package layout;
- data access and environment rules;
- label store contract;
- evidence/event layer contract;
- feature table contract;
- dataset builder contract;
- model, calibration, evaluation, scoring, and delivery separation;
- serving mode and training-serving parity check;
- monitoring, drift, retraining trigger, and feedback-loop policy;
- artifact and manifest policy;
- leakage controls;
- time/as_of policy;
- non-blocking validation work.

May advance to implementation planning when:

- hard boundaries are accepted by the user;
- unresolved EDA/experiment items are tracked and non-blocking;
- implementation stages have clear inputs and outputs.

Must not advance when:

- code/artifact ownership is ambiguous;
- database/network/environment rules are missing;
- model score and business decision are conflated;
- training-serving parity is undefined or no monitoring/retraining plan exists for a production model;
- no manifest/versioning policy exists.
