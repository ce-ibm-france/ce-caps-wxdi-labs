# AGENTS.md

This file provides guidance to agents when working with code in this repository.

## What This Repository Is

A **pure documentation/lab-content** repo — no code, no build system, no tests. All content is Markdown + JSON config files. There is nothing to compile, lint, or run.

## Repository Layout Rules

- Each lab lives in its own **top-level subfolder** (e.g. `StreamSets/`).
- Every lab subfolder must have: `README.md`, `setup/` (tutor guide), and at least one `module*/` folder.
- The root `README.md` is the repo entry point — it links to each lab subfolder.
- Each lab `README.md` must open with `[← Back to all labs](../README.md)`.
- `.gitignore` lives at the **repo root**, not inside lab subfolders.

## Content Conventions (Non-Obvious)

### Naming
- StreamSets pipeline tables are named `fx_eurusd_<YOUR_INITIALS>` — every mention of the target table must include the `_<YOUR_INITIALS>` suffix (participants personalise it to avoid collisions).
- Consumer group must be `streamsets-lab-<YOUR_INITIALS>` — same reason.
- Kafka schema subject is always `<topic>-value` (e.g. `fx_rates-value`) — never just the topic name.

### Screenshot placeholders
Screenshots do not yet exist. Use this exact placeholder pattern — do **not** embed images that aren't in `images/screenshots/`:
```markdown
> 📸 **Screenshot placeholder** — `images/screenshots/<filename>.png`
> *Alt text describing what the screenshot should show.*
```
Real screenshot references (files that exist) use standard `![alt](path)` syntax.

### Connector config (`alphavantage-generator.json`)
Despite the name prefix, this connector uses **Confluent DatagenConnector** (not AlphaVantage API). It generates synthetic multi-currency exchange rate data for 10 `from_currency` × 10 `to_currency` combinations (`rate` range 0.05–200.0) at `max.interval = 30000 ms` (30 sec). Do not rename it — the filename is referenced throughout `TUTOR_SETUP.md` and the `.gitignore` pattern (`alphavantage-generator-*.json`).

`from_currency` and `to_currency` draw from **disjoint lists** — `from_currency` is always one of EUR, GBP, CHF, CAD, AUD, NZD, SEK, NOK, DKK, SGD; `to_currency` is always one of USD, JPY, CNY, INR, BRL, MXN, ZAR, HKD, KRW, TRY. Same-pair records (e.g. EUR/EUR) are **structurally impossible**. Do not merge the two lists back into one single shared list.

### `/kafkaTimestamp` field
StreamSets automatically exposes the Kafka broker-assigned timestamp as a record header attribute named `timestamp`. It is **not** a field in the Avro payload — do not add it to the schema. The Expression Evaluator accesses it via `record:attribute('timestamp')` and promotes it to a `/timestamp` record field.

### Presto JDBC URL SSL
A common lab failure is a missing SSL parameter in the Presto JDBC URL. Module 2 notes `?SSL=true` or `?sslEnabled=true` depending on version — keep both variants in troubleshooting sections.

## Images
- `images/` folder is inside each lab subfolder (`StreamSets/images/`), not at the repo root.
- Screenshots go in `images/screenshots/` with the naming pattern `m1-<NN>-<description>.png` (module 1) or `m2-<NN>-<description>.png` (module 2); tutor setup screenshots have no prefix.

## IBM Data Integration Instance
The lab targets `https://ca-tor.dai.cloud.ibm.com` — always use this exact URL, do not substitute other regions.
