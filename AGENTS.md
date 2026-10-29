# Agent instructions

## Mission and phase boundary

Build AI Coding Evaluation Tracker using synthetic data only. Phase 1 is documentation only. Do not implement business code until the user starts the implementation phase. Do not report planned behavior as working.

## Start every task

1. Read this file, README.md and the local task_state.md.
2. Read the assigned public issue and its acceptance criteria.
3. Read docs/product-spec.md, docs/architecture.md and the relevant sections of docs/acceptance.md.
4. Read docs/team.md for role boundaries and docs/process.md for handoff and evidence rules.
5. Check local pitfall_log.md and Git status. Preserve unrelated changes.
6. State the active role and exact acceptance conditions before editing.

## Modular context

- Product authority: [product spec](docs/product-spec.md)
- Data and API authority: [architecture](docs/architecture.md)
- Roles and handoff: [team](docs/team.md)
- Tracker and Git workflow: [process](docs/process.md)
- Tests and observable outcomes: [acceptance](docs/acceptance.md)
- Course criteria: [rubric mapping](docs/rubric-mapping.md)
- Deploy, rollback and optional telemetry: [operations](docs/operations.md)
- Current evidence: [ledger](docs/evidence/README.md)

## Implementation constraints

Keep one backend process and one SQLite database. Prefer explicit functions and small components over frameworks for hypothetical scale. Use database transactions and foreign keys. Validate all input on the server. Freeze criteria once referenced by an evaluation; create a new task for changed criteria. Preserve finalized evaluations and Debug history.

Do not import company data, personal evaluation records, credentials or unrelated local files. Do not execute or fetch user-supplied evidence references. Never automate WorldQuant BRAIN submissions. No automatic installation, paid hosting purchase or credential changes without the user's authorization.

## Evidence and completion

For each implemented feature, run meaningful tests against its changed behavior. Record commit, environment, command, exit code, observed result and artifact in docs/evidence/. A screenshot proves only what is visible. A passing backend test does not prove browser behavior or a live deployment.

Use PLANNED, IN_PROGRESS, IMPLEMENTED_UNVERIFIED, VERIFIED, FAILED or BLOCKED. Only VERIFIED may satisfy acceptance; a claimed pass without reproducible evidence remains UNVERIFIED. Recheck affected evidence after relevant changes.

## Session handoff

Update the local task_state.md with completed work, evidence and the next task. Log recurring or multi-round problems in pitfall_log.md. These machine-local files are excluded from Git; publish sanitized engineering evidence separately. Do not copy personal global memory into the public repository.
