# AI Coding Evaluation Tracker

A small full-stack application for keeping coding evaluation tasks, scoring rules, model results and Debug retests together. Spreadsheets often lose the relationship between a criterion, its evidence and later fixes. This tracker will preserve that relationship and distinguish missing evidence from a measured failure.

**Phase 1: specification only. No application, test suite or deployment exists yet.** All runtime acceptance conditions are UNVERIFIED. This repository uses synthetic examples only.

## Expected behavior

An evaluator creates a task and its criteria, starts an evaluation for a fictional model version, records criterion results with evidence references, and finalizes the evaluation. A subsequent Debug round creates a new evaluation linked to the original. Earlier results remain readable and unchanged. Partial results show coverage and a provisional total; they never imply that untested criteria passed.

## Planned stack and scope

React with TypeScript and Vite; FastAPI with Python; SQLite; Pytest; Vitest and React Testing Library; Playwright for one real browser journey; Docker Compose; GitHub Actions. One evaluator, no external model API, no paid AI inference, no file uploads, no multi-tenant permissions.

## Reviewer guide

- [Product specification](docs/product-spec.md)
- [Architecture and API contract](docs/architecture.md)
- [Acceptance conditions and test plan](docs/acceptance.md)
- [Official rubric mapping](docs/rubric-mapping.md)
- [Public task tracker](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues)
- [Backlog and dependencies](docs/backlog.md)
- [Agent instructions](AGENTS.md), [roles](docs/team.md), [working process](docs/process.md)
- [Deployment and operations plan](docs/operations.md)
- [Evidence ledger](docs/evidence/README.md)
- [Course submission checklist](docs/submission.md)

## Planned commands

These are interface contracts for later implementation, not commands available in Phase 1.

Local startup: `docker compose up --build -d`; UI: `http://localhost:8080`.
All test layers: `docker compose --profile test run --build --rm verify`.
Stopping without data deletion: `docker compose down`.

The verify service must prepare an isolated test database, run backend unit/integration tests, frontend tests and the browser journey, and exit nonzero on any failure. A reviewer must not have to run migrations or seed commands manually.

## Course baseline

Planning started on 2026-10-30 (Asia/Shanghai). The [official project page](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp/blob/526d9c12506c159e7a8f3cef30e5de1eeaec287b/project/README.md) was verified on that date. The detailed source version and scoring plan are in the rubric mapping. Passing the project and obtaining a course certificate are distinct milestones; the course platform controls final eligibility.
