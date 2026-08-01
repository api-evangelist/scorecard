---
name: Build a Scorecard evaluation testset
description: Create a project and a testset, then load testcases to evaluate an AI agent.
api: openapi/scorecard-openapi-original.yml
operations: [createProject, createTestset, createTestcases, listTestcases]
---

# Build a Scorecard evaluation testset

Use the Scorecard API to stand up an evaluation dataset for an AI agent.

## Auth
Send `Authorization: Bearer <SCORECARD_API_KEY>` on every request. Keys start with `ak_`.
Base URL: `https://api2.scorecard.io/api/v2`.

## Steps
1. **Create a project** — `POST /projects` (`createProject`) with `name` and `description`. Keep the returned project `id`.
2. **Create a testset** — `POST /projects/{projectId}/testsets` (`createTestset`) under that project. Keep the testset `id`.
3. **Load testcases** — `POST /testsets/{testsetId}/testcases` (`createTestcases`) to add multiple testcases in one call.
4. **Verify** — `GET /testsets/{testsetId}/testcases` (`listTestcases`); page with `limit` + `cursor`, reading `data`, `nextCursor`, and `hasMore`.

## Conventions
- List responses are cursor-paginated: `{ data, nextCursor, hasMore, total }`. Pass `nextCursor` back as `cursor`.
- Errors return `{ code, message, details }`; `401` means a missing/invalid `ak_` key.
