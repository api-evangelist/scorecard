---
name: Run a Scorecard evaluation and record scores
description: Define a metric, create a run, write records, and upsert scores for an AI agent evaluation.
api: openapi/scorecard-openapi-original.yml
operations: [createMetric, createRun, createRecord, upsertScore, listRecords]
---

# Run a Scorecard evaluation and record scores

Execute an evaluation over a testset and persist the results.

## Auth
Send `Authorization: Bearer <SCORECARD_API_KEY>` (key prefix `ak_`).
Base URL: `https://api2.scorecard.io/api/v2`.

## Steps
1. **Define a metric** — `POST /projects/{projectId}/metrics` (`createMetric`). Metric structure depends on its `evalType` and `outputType`. Keep the metric config `id`.
2. **Create a run** — `POST /projects/{projectId}/runs` (`createRun`), referencing the testset (and optionally a system/system-version). Keep the run `id`.
3. **Write records** — `POST /runs/{runId}/records` (`createRecord`) for each evaluated result. Record bodies may be up to 15MB.
4. **Upsert scores** — `PUT /records/{recordId}/scores/{metricConfigId}` (`upsertScore`). This is idempotent: an existing score for that record/metric is updated, not duplicated. The score must conform to the MetricConfig schema or you get a validation error.
5. **Read results** — `GET /runs/{runId}/records` (`listRecords`), which returns each record with its scores; page with `limit` + `cursor` and filter with `tags`.

## Conventions
- `upsertScore` is idempotent — safe to retry.
- List responses are cursor-paginated `{ data, nextCursor, hasMore, total }`.
- Errors return `{ code, message, details }`.
