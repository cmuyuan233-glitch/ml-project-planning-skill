# Data Safety Gate

Use this before reading real datasets, schemas containing sensitive fields, samples, exports, logs, or before running EDA/code over user data.

Planning can proceed without real data. Real data handling requires an explicit boundary first.

## When This Gate Applies

Apply this gate when:

- data availability mode is `data_in_hand` or `data_partial`;
- the user asks the agent to inspect files, database tables, logs, samples, or schemas;
- the agent will run code, EDA, profiling, joins, labeling, feature generation, or exports over user data;
- the agent may call external tools, APIs, hosted notebooks, model hubs, or remote compute with user data.

If the user only describes data and no real data is read or processed, record `data_described` and continue with assumptions plus validation hooks.

## Safety Boundary

Before touching data, produce or confirm a short `Data Safety Note`:

| Item | Required Decision |
|---|---|
| Approved inputs | Exact files, folders, tables, schemas, or APIs that may be read. |
| Processing location | Local machine, warehouse, controlled cloud job, or other approved environment. |
| External transfer rule | Whether data may leave the approved boundary. Default: no upload/export to external services unless explicitly approved. |
| Raw display policy | Whether raw rows may be shown. Default: prefer schema, counts, aggregates, and redacted examples. |
| Sensitive fields | Direct identifiers, protected attributes, secrets, credentials, PHI/PII/financial fields, and known proxies. |
| Write policy | Source data is read-only; derived outputs go to a named output location. |
| Logging policy | Do not print secrets, tokens, full identifiers, or unnecessary raw records into logs, prompts, commits, or PRs. |
| Approval owner | Who accepted the data boundary and any residual risk. |

Unknowns become open questions. If the user cannot answer, fall back to `data_described` planning rather than inspecting data.

## Operating Rules

- Read only approved paths/tables; do not recursively discover unrelated data.
- Use least-data-first analysis: schema, row counts, missingness, distributions, and aggregates before raw samples.
- Limit raw samples to the smallest useful size, redact identifiers, and avoid pasting sensitive rows into final answers.
- Do not overwrite source data. Write derived artifacts to a clearly named output folder.
- Do not upload data, derived row-level data, embeddings, logs, or artifacts to external services unless the user explicitly approved that route.
- Prefer read-only SQL and bounded queries when using databases.
- Stop and ask for approval before handling regulated, people-affecting, credential-bearing, or legally restricted data when the allowed boundary is unclear.

## Completion Contract

The gate is complete when the `Data Safety Note` names the approved inputs, processing boundary, raw display rule, external transfer rule, sensitive fields, write/output policy, and approval owner.

The gate is not needed when no real data, schema, sample, export, or user-data execution is involved.