# If I Were to Create a Mobile Search

[![Report preview](assets/report-preview.png)](https://claude.ai/artifact/1KJzmLteTKNPYHVLYaLqLb)

**Live report:** https://claude.ai/artifact/1KJzmLteTKNPYHVLYaLqLb (same sanitized content as [`index.html`](index.html))

> **Confidentiality note.** Nothing here uses a real person's name or a real ticket number, but the intellectual ideas are mine and were created by me.

A Paxel-style profile of the backend I built as a new hire: one search and profile API over every kind of healthcare contact, built for a greenfield mobile CRM app while two frontend engineers built the UI on top of it. It focuses on the system design and its impact.

## Contents

| File | What it is |
| --- | --- |
| [`index.html`](index.html) | The full profile report: impact summary, what I'd build, backend-to-frontend dependency table, five dimensions of outcomes and actions, and challenges. Open it in a browser. |
| [`TRANSCRIPT.md`](TRANSCRIPT.md) | Sanitized transcript of the Claude Code session that produced the report: messages plus a one-line log of each analysis step. Raw tool output and source code are left out. |

## Impact

- **One search for every contact.** Reps find patients, caregivers, prescribers, clinics, office staff, and program contacts from a single search box instead of separate lookups.
- **Permission-safe by design.** Access is resolved before any SQL is built, so hidden records never leak through results, counts, or paging.
- **HIPAA masking in one place.** Masking happens at the source with shared sentinels, so the backend and the mobile app always agree on what is hidden.
- **The UI was never blocked.** Each backend layer landed before the mobile screen that needed it, so frontend work kept moving in parallel.
- **A contract that evolved safely.** Additive changes and staged deprecations let the API change while the app was being built on it.
- **Ownership as a new hire.** Learned a multi-tenant, regulated platform through smaller fixes, then took ownership of the backend for a new product area.

## Dimensions

1. **Engineering: backend depth across entities.**
   - **Normalized every data shape.** One CTE per entity turns relational rows, JSONB documents, and nested caregiver arrays into one common row, so matching, ranking, and paging are written once.
   - **Authorized before querying.** An access guard decides which types and links a role may see before any SQL is built, so new access rules plug in without touching the query.
2. **Product instinct and execution: iterating on requirements.**
   - **Refined ranking with real use.** Case-insensitive order, exact matches first, last-name sort, and primary links first, so reps see the most relevant contact at the top.
   - **Kept the contract clean.** A shared `Page` interface and staged deprecations meant the mobile client never broke.
3. **Simplification: one source of truth for HIPAA masking.**
   - **Masked at the source.** Masking moved into the SQL layer, so paging, sorting, and links all see the same value.
   - **Shared, collision-proof sentinels.** Backend and frontend import the same mask constants, so the UI can always tell hidden from empty, and teammates built on it right away.
4. **Verification: tests, end-to-end, and load.**
   - **Rules checked against a real database.** Permission and masking rules run against a Postgres test database with a realistic tenant fixture.
   - **Flows covered end to end.** Mobile Playwright flows exercise the API, and latency timers sit in the request path. Load-test results are still to be added.
5. **Planning: thin, safe slices.**
   - **Built bottom-up.** Data access, then API, pagination, permissions, masking, and profiles, so each layer landed before the UI needed it.
   - **Kept the stack stable.** Small, focused PRs and nothing on the search stack had to be reverted.

## Method

The reading comes from the code and git history of the search stack in the original repository. Every stand-in reference maps one-to-one to a real item, so the original can be verified on request.
