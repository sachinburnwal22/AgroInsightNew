# Implementation Plan: Replace the AgroInsight Laravel API with Django

**Project**: AgroInsight
**Scope**: Replace the Laravel/PHP backend with a Python/Django backend while retaining the existing product behavior and frontend.
**Status**: Planning only. No application code, database, or secret files have been changed.

## Technical Framework

- **Current API**: PHP 8.2+, Laravel (the root/backend README badges say Laravel 11; `backend/composer.json` requires Laravel 12), Sanctum
- **Target API**: Python and Django; exact supported Python/Django versions are to be selected during implementation
- **Frontend**: Next.js 16.2, React 19, TypeScript
- **Realtime service**: Separate Node.js/Express/Socket.IO service on port 3001
- **Database**: Laravel config defaults to SQLite and `backend/database/database.sqlite` exists, but the effective connection cannot be established without inspecting the prohibited `backend/.env`; Laravel also contains MySQL, MariaDB, PostgreSQL, and SQL Server connection options
- **External integrations**: Gemini API and PIB RSS feeds, evidenced by `GeminiService` and `NewsSyncService`

## Overview

Replace the Laravel backend with a maintainable Django/Python service while preserving AgroInsight's current UX and backend contract. The migration will first characterize the existing API and data model, then establish Django and migrate feature behavior in bounded groups. The frontend and socket server remain in place; frontend changes are limited to what is needed to continue using the same API behavior. The original database and secret files remain protected throughout the work.

## Evidence and Current Scope

Inspection of `backend/routes/api.php` found **58 HTTP route declarations** (plus a middleware group). They cover:

- Health/test, regions, analytics, crops, dashboard, irrigation, spraying, soil, weather, reports, and alerts.
- Registration, login, logout, user profile read/update; authenticated routes use `auth:sanctum`.
- Communities, membership, posts, comments, likes, community chat, invitations, notifications, co-explore sessions, and location updates.
- Shops, products, cart, payment order creation/verification, and AI market recommendations.
- Live/trending/location-based news, article bookmarks, AI summaries, schemes, explanations, and government alerts.
- Advisor weather, crop recommendations, and product recommendations.

The backend has **22 Eloquent model files**, migrations defining **34 tables** (including framework tables), seeders, and the `GeminiService` / `NewsSyncService`. The checked-in Laravel feature and unit test files are examples only; they do not characterize the API. The SQLite database files were identified by filename and file metadata only; their contents were not read.

The frontend's `API_BASE_URL` defaults to `http://127.0.0.1:8000`, appends `/api/...` paths, and its auth context stores a bearer token in `localStorage` and sends it as an Authorization header. Backend CORS currently allows all origins, methods, and headers, with credentials disabled. The socket server is independent, has its own health endpoint, and implements Socket.IO invitation, room, movement, chat, interaction, and WebRTC signaling events; it is out of scope for replacement.

## Migration Impact Summary

| Component | Current | Target | Required compatibility |
|---|---|---|---|
| Backend REST API | Laravel/PHP | Django/Python | Same paths, verbs, payloads, response/error shapes |
| Authentication | Sanctum bearer tokens | Django bearer-token behavior | Login/register/logout and protected-route behavior |
| Persistence | Laravel-managed DB | Django-managed data access | Preserve existing records; no destructive source changes |
| Web client | Next.js/React | Retained | Preserve UX; adjust API integration only if required |
| Realtime | Node/Socket.IO | Retained unchanged | Preserve port, events, and socket behavior |
| Documentation/launch | Laravel-oriented | Django-oriented | Update root, backend, and frontend guidance as needed |

## Execution Constraints and Safety Gates

1. Capture the route, request/response, authentication, CORS, and feature behavior before replacement. Build tests against observed behavior; do not infer missing contracts from documentation alone.
2. Preserve the `/api` route prefix and current status codes, JSON shapes, validation behavior, and auth boundaries wherever observed. Cover the full route inventory, including multipart profile updates and external-service fallbacks.
3. Keep the existing Next.js experience and Socket.IO service intact. Only change frontend API integration where tests show it is necessary.
4. Do not read, print, copy into logs, or expose `backend/.env`. Do not overwrite, delete, or move it without explicit user confirmation. Do not display secrets in plan/test output.
5. Do not read or modify the original SQLite database or other user data in place. Before any data migration, confirm the active source database and destination, take a recoverable byte-for-byte backup/copy, perform migration on a separate working copy or destination, and verify schema and record counts without disclosing record contents. Do not drop or rewrite the source database. Include foreign keys, join tables, seed/reference data, and relevant token records in the mapping.
6. Preserve existing authentication where feasible. Determine whether existing Sanctum tokens can be safely validated by the Django implementation from the copied schema; if not, stop before invalidating them and ask for approval to require users to sign in again.
7. Remove Laravel application/runtime artifacts only after Django/API/frontend verification passes. Leave `backend/.env` and database/user-data files untouched by default. This means final removal of every Laravel-related artifact is conditional on the user's confirmation for any protected file that would otherwise need deletion or relocation.
8. Do not deploy, provision infrastructure, or execute the plan in this planning phase.

## Validation and Completion Criteria

- Django tests cover every migrated endpoint group, success/error cases, authorization boundaries, and response contracts.
- Verify API route and auth parity, CORS behavior, serialization and relations, and service failure behavior without requiring live secrets.
- Verify data migration from an approved copy with schema/record-count reconciliation and referential-integrity checks; retain the untouched original.
- Run backend tests and available backend checks, plus frontend lint/build and focused API integration checks. Keep Socket.IO smoke checks focused on confirming the unchanged service remains compatible.
- Confirm setup/run instructions launch Django, and that root/backend/frontend documentation no longer instructs users to start Laravel.
- Only after the preceding gates pass, retire PHP/Laravel application artifacts while retaining protected data/secret files.

## Open Questions & Decisions Requiring Confirmation

- [ ] Which database is actually active for the Laravel application, and what destination should Django use? The Laravel config defaults to SQLite, but the active override is unknown because `backend/.env` was intentionally not read. Confirm before any database access or migration.
- [ ] Preserve the existing Sanctum sessions if technically feasible, or approve a one-time re-login if token compatibility cannot be safely maintained.
- [ ] After successful cutover, should `backend/.env` remain as an opaque preserved file, or may it be archived/removed? It will not be read, moved, overwritten, or deleted without explicit approval.
- [ ] The plan is Python/Django, but the available modernization task schema accepts a free-form language string while the Java/.NET execution routing described by the planning workflow does not support Python. Route execution through a Python-capable rearchitecture worker before implementation; do not mislabel the project as Java or .NET.

The executable task graph and task-level success criteria are in `.metadata/tasks.json`.
