# Skill Test Cases

Use these prompts to test whether the skill asks the right early questions and prevents premature implementation.

## Case 1: Vague Business Model

Prompt:

```text
I want to predict whether customers are good or bad. Help me build a model.
```

Expected behavior:

- Ask what business decision uses the score.
- Ask what "good" and "bad" mean.
- Ask prediction unit and timing.
- Do not select a model yet.

## Case 2: Churn Prediction

Prompt:

```text
We want to predict which users will churn next month so our success team can intervene.
```

Expected behavior:

- Define user + as_of_date prediction unit.
- Define churn window.
- Discuss intervention leakage.
- Discuss recall vs precision tradeoff and team capacity.

## Case 3: Credit Default

Prompt:

```text
We need a credit risk model. We have application data, repayment history, and manual approval notes.
```

Expected behavior:

- Separate application-time data from future repayment outcomes.
- Treat manual approval notes with caution.
- Define false positive/false negative cost.
- Require time split/OOT evaluation.

## Case 4: Demand Forecasting

Prompt:

```text
Predict SKU demand for stores for the next eight weeks.
```

Expected behavior:

- Define store + SKU + forecast week grain.
- Define forecast horizon.
- Separate known future calendar/promotions from unknown future signals.
- Use time-series evaluation.

## Case 5: Content Moderation

Prompt:

```text
Build a classifier to automatically block unsafe posts.
```

Expected behavior:

- Clarify policy categories and labels.
- Discuss human review vs automatic block.
- Identify high false-positive cost.
- Require calibration/threshold policy separation.

## Case 6: Adversarial Leakage

Prompt:

```text
We can train today's model using human review results, previous model scores, and user behavior from the next 30 days.
```

Expected behavior:

- Flag human review fields, prior model scores, and future 30-day behavior as leakage risks unless specifically time-safe.
- Require `as_of_date`.
- Separate metadata from model features.

## Case 7: Private / Undisclosed Data

Prompt:

```text
I want to predict equipment failure, but the data is confidential and I can't share it.
```

Expected behavior:

- Do not stop or demand the data.
- Set data availability mode to `data_described` (or `data_absent` if it does not exist yet).
- Plan on stated assumptions and record them in an assumptions register with validation hooks.
- Mark feasibility (label volume, base rate) as `need_data`, not a blocker.
- Produce a full plan that the first real data pull can validate or rescope.

## Case 8: No Data Yet / Cold Start

Prompt:

```text
We have an idea for a model but we haven't collected any data yet.
```

Expected behavior:

- Set mode `data_absent`.
- Produce a data acquisition + labeling plan (what to log, how to label, volume/budget).
- Keep the rest of the plan as hypotheses with validation hooks.
- Do not pretend the model can be evaluated before data exists.

## Case 9: Intervention Framed As Prediction (Causal Trap)

Prompt:

```text
Predict which customers will churn so we can send them a discount, and just build the most accurate churn classifier.
```

Expected behavior:

- Flag the predict-vs-act gap: the action makes this a causal/uplift question, not pure prediction.
- Recommend a predictive baseline now and an uplift experiment (X-005) once treatment/control data exists.
- Define the non-ML heuristic baseline to beat.

## Case 10: Existing Project Reframe

Prompt:

```text
We have a v1 model but new labels and new data sources. Help design v2.
```

Expected behavior:

- Audit v1 assumptions rather than inheriting them.
- Define what remains fixed vs needs EDA/experiments.
- Create a boundary register.
- Preserve or revise engineering architecture based on new labels.
