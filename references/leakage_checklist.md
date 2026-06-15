# Leakage Checklist

Use before admitting features into a dataset.

## Label Leakage

Block fields that directly or indirectly encode the label:

- label value
- label source
- human review decision
- conflict flag
- audit outcome
- trust tier when derived from label source
- target status created after the prediction time

## Future Leakage

Block or time-filter:

- events after `as_of_date`
- outcomes after prediction time
- future transactions or behavior
- future profile updates
- future model scores
- future human interventions

## Source Leakage

Watch for sources created by the same process that generated labels:

- review workflow exports
- analyst annotations
- post-hoc correction tables
- delivery outputs

## Model Leakage

Block:

- previous model scores unless explicitly part of a time-safe stacked model
- downstream decision tiers
- calibrated probabilities from future models
- explanation outputs from target model

## Operational Leakage

Block:

- fields only available after manual action
- fields collected because the previous model selected the row
- fields representing business intervention

## Required Controls

- Use feature allowlist/blocklist.
- Carry `as_of_date`.
- Report dropped future evidence count.
- Keep metadata separate from model input.
- Version feature columns.
