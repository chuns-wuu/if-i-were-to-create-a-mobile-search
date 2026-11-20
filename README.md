# If I Were to Create a Mobile Search

[![Report preview](assets/report-preview.png)](https://claude.ai/artifact/1KJzmLteTKNPYHVLYaLqLb)

**Live report:** https://claude.ai/artifact/1KJzmLteTKNPYHVLYaLqLb (same sanitized content as [`index.html`](index.html))

> **Confidentiality note.** This repository is a sanitized write-up of work done on a private company codebase. The company ("MedLink"), teammates ("Aaron Brooks", "Jason Goh"), tickets (`MOB-101`, `PLT-101`, ...), commits (`c001`, ...), PRs (`PR-01`, ...), database tables, files, and code identifiers are all stand-in names that map to the real items. Counts are rounded to show rough scale. No source code, customer data, or real identifiers from the original repository are included, and they remain confidential.

A Paxel-style profile of the backend I built as a new hire: one search and profile API over six kinds of healthcare contacts, built for a greenfield mobile CRM app while two frontend engineers built the UI on top of it.

## Contents

| File | What it is |
| --- | --- |
| [`index.html`](index.html) | The full profile report: summary metrics, team-scope chart, backend-to-frontend dependency table, five scored dimensions, and challenges. Open it in a browser. |
| [`TRANSCRIPT.md`](TRANSCRIPT.md) | Sanitized transcript of the Claude Code session that produced the report: messages plus a one-line log of each analysis step. Raw tool output and source code are left out. |

## At a glance

| Signal | Value (rounded) |
| --- | --- |
| Merged PRs on the search stack | ~27, covering ~25 tickets, Jan to Apr 2025 |
| Lines changed | ~+12k / −4k |
| Share of surviving lines I wrote | ~90% of ~8.5k |
| Automated test cases | 120+, about 2 test lines per production line |
| Scope | 6 contact types, ~7 Postgres tables, ~14 link pairs, 3 GraphQL queries |
| Ramp-up | 8 weeks from start date to first core PR |

## Dimensions

1. **Engineering: backend depth across entities.** One query pipeline normalizes relational rows, JSONB documents, and nested caregiver arrays into a single ranked, paginated result. An access guard decides which types and links a role may see before any SQL is built.
2. **Product instinct and execution: iterating on requirements.** Ranking went through four refinements: case-insensitive order, exact matches first, last-name sort, and primary links first. Search by external ID was added. Three API contract changes shipped without breaking the mobile client.
3. **Simplification: one source of truth for HIPAA masking.** Masking moved into the SQL layer, and the backend's local mask strings and the frontend's copies were replaced by shared sentinel constants that can't collide with real data. Three teammate PRs built on it within four weeks.
4. **Verification: tests, end-to-end, and load.** Unit and integration coverage is strong and runs against a Postgres test database. Mobile Playwright flows exercise the API end to end. Latency timers sit in the request path. Load-test results are still to be added.
5. **Planning: thin, safe slices.** The build order was data access, then API, then pagination, permissions, masking, and profiles, with a median gap of 1 day between core PRs and no reverts on the stack.

## Method

Metrics come from `git log --shortstat` and `git blame -w` on the core files of the search stack in the original repository. Frontend comparisons count commits on the mobile UI paths only. Every stand-in reference maps one-to-one to a real item, so the original can be verified on request.
