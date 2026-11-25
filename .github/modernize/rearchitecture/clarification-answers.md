---
schema: clarification-answers/v1
status: submitted
generated_at: "2026-11-24T05:54:06.228Z"
scope: [backend, generic]
questions_file: clarification-questions.json
---

# Rearchitecture Clarification Answers

## ⚙️ Backend

- **B1** Which Django and Python major versions should the implementation use?
  - answer: _none_
  - source: skipped (required, no default — blocking gap)
- **B2** Which policy should govern existing REST API compatibility?
  - answer: must preserve
  - source: user
- **B3** How should existing application data be migrated from Laravel to Django?
  - answer: new schema
  - source: user
- **B4** How should Django authentication preserve the Laravel Sanctum bearer-token behavior?
  - answer: preserve existing auth mechanism; detect from codebase
  - source: user
- **B5** What non-functional performance target should replace the Laravel baseline?
  - answer: match current production baseline; no regression
  - source: user

## 📋 General

- **G1** What outcome must verify the Laravel-to-Django replacement is complete?
  - answer: feature parity with the current system; all existing user-facing behaviors preserved
  - source: user
- **G2** Which components or behaviors must remain untouched?
  - answer: Next.js frontend and Socket.IO service remain unchanged; only necessary frontend API integration may change
  - source: user
- **G3** What must happen to existing test suites during migration?
  - answer: must pass
  - source: user
- **G4** Where should the Django backend replacement be written?
  - answer: in-place
  - source: user
- **G5** Add any other constraints, exclusions, compliance rules, or dependencies that should govern this migration.
  - answer: Back up user data safely; never expose backend/.env or database secrets; do not modify the original SQLite database in place; migrate from an approved copy/destination; validate data and API parity before removing Laravel files; preserve the Next.js frontend and Socket.IO server.
  - source: user
