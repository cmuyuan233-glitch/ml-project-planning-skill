# Worked Example: Subscription Churn (Private Data)

A filled-in, end-to-end pass to show what "good" looks like. It deliberately uses a `data_described` situation: the user cannot share data because it is private, so the plan runs on stated assumptions with validation hooks. This is the common real-world case, and it shows the skill's slide-thinking behavior.

> Prompt: "We want to predict which users will churn next month so our success team can reach out. I can't share the data, but I can describe it."

## Project Definition Brief (filled)

- **Business decision**: Customer-success team contacts at-risk users; capacity is ~500 outreaches/week.
- **Problem framing**: ML candidate, but the *action* is an intervention -> the honest frame is causal/uplift ("who is retained if we reach out"), not pure churn prediction. We will start with a predictive baseline and flag uplift as experiment X-005. Non-ML baseline: "contact users whose logins dropped >50% month-over-month."
- **Prediction unit**: `user_id + as_of_date` (one row per active user per weekly scoring date).
- **Prediction timing**: scored each Monday; only data available before that Monday may be used. `as_of_date` = scoring Monday.
- **Output contract**: calibrated churn probability; business decision (who to contact) is separate from the score.
- **Label**: positive = no paid activity in the 30 days after `as_of_date`; negative = retained; unknown = users who paused for billing reasons. Labels are *derived from logged billing events* (exist already, gold).
- **Error cost**: false negative (missed churner) loses revenue; false positive wastes outreach capacity. FN more expensive, but capacity caps how many FPs we tolerate -> precision@500 matters.
- **Success threshold**: must beat the login-drop heuristic on precision@500 and lift retained revenue enough to justify the team's time; abandon if it cannot beat the heuristic.
- **Evaluation priority**: primary precision@500 (capacity-bounded), secondary AUC + calibration; slices by plan tier and tenure; OOT evaluation by month.
- **Data availability**: `data_described`. Assumed ~80k active users, ~4% monthly churn base rate, 24 months history. Plausibly enough positives (assumption D-001).
- **Fairness/compliance**: low-stakes internal model; no protected-attribute decisions; note recorded, formal fairness audit N/A.
- **Non-goals**: not building the outreach tooling; not modeling win-back of already-churned users.

## Data Requirements Spec (filled, `data_described`)

| Need | Purpose | Grain | Min fields | Source (assumed) | History | Assumption? |
|---|---|---|---|---|---|---|
| Billing events | label + features | user x day | event type, amount, ts | billing DB | 24 mo | yes |
| Product usage | features | user x day | login, key actions, ts | event log | 24 mo | yes |
| Plan/profile | features (slices) | user | tier, tenure, region | accounts DB | current | yes |

Assumptions register (load-bearing):

| ID | Statement | Risk if wrong | Validation hook | Status |
|---|---|---|---|---|
| A-1 | ~4% monthly churn, ~80k users -> enough positives | underpowered model | label-count EDA on first data pull | assumed |
| A-2 | usage events have reliable timestamps before `as_of_date` | time leakage / unusable features | time EDA: event-time availability | assumed |
| A-3 | billing-derived labels are unambiguous | wrong target | label conflict/age EDA | assumed |

## Boundary Register (excerpt)

| ID | Question | Status | Note |
|---|---|---|---|
| B-001 | Prediction unit | `fixed_now` | `user_id + as_of_date`, weekly |
| B-002 | `as_of_date` policy | `fixed_now` | use only pre-Monday data |
| D-001 | Enough positives? | `need_data` | assume yes (A-1); confirm on first pull |
| D-002 | Usage features exist at cutoff? | `need_data` | assume yes (A-2); time EDA |
| X-005 | Predictive vs uplift targeting | `need_experiment` | needs treatment/control outreach data |

## EDA Plan (becomes validation hooks while data is absent)

Because data is described-only, each EDA item is a hook to run on the first real pull: label-count/base-rate EDA (A-1), event-time availability EDA (A-2), label conflict/age EDA (A-3). None block planning now.

## Feature Plan (excerpt)

| Feature | Class | Why | Leakage / serving note |
|---|---|---|---|
| logins_last_7d / 30d | baseline | engagement signal | events strictly before `as_of_date`; available online |
| days_since_last_login | baseline | disengagement | as-of safe |
| support_tickets_open | experiment | possible churn signal | confirm available at serving time |
| outreach_already_sent | blocked | leaks the intervention | post-decision; metadata only |
| future_cancellation_flag | blocked | the label itself | never a feature |

## Experiment Matrix (excerpt)

- X-000: login-drop heuristic vs logistic-regression baseline; adopt ML only if it beats heuristic on precision@500.
- X-001: logistic regression vs gradient boosting; same split/labels/features.
- X-005: predictive targeting vs uplift targeting (only once treatment/control outreach data exists).

## Architecture (excerpt)

Weekly batch scoring; features rebuilt from the same as-of code in train and serve (parity); monitor input drift and precision@500 as labels mature (30-day label latency); retrain monthly or on drift; outreach decision layer separated from the score.

## Why this is "ready enough"

Nothing was inspected, yet the plan is concrete, leakage-safe, and falsifiable: every unverifiable claim is an assumption with a hook, and the first data pull either validates or rescopes the project. That is the slide-thinking behavior the skill should always produce when data is private or absent.
