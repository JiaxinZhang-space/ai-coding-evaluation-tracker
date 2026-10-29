# Working process

## Public tracker

GitHub Issues is the public source of task scope and acceptance conditions. docs/backlog.md is the dependency map. Use issue headings Scope, Acceptance criteria, Dependencies, Evidence and Status. GitHub Projects is optional; no second tracking service is needed.

States: PLANNED -> IN_PROGRESS -> IMPLEMENTED_UNVERIFIED -> VERIFIED. FAILED returns to implementation; BLOCKED records the missing dependency. An open issue may hold any nonfinal state. Close an issue only after all required acceptance items have evidence.

At start, post a comment naming the active role, the issue read, the acceptance IDs and the plan. At completion, post the commit and exact test evidence. This demonstrates actual agent use of the tracker. Creating empty issues alone does not demonstrate such use.

## Git from the first day

The initial planning commit must predate application code. Use small issue-linked branches such as feat/p03-evaluation-records and pull requests targeting main. Commit messages reference the issue. Do not squash away every useful step or fabricate historical activity. Before pushing, check staged files for secrets, private logs, databases and unrelated material.

Phase 1 can commit directly to main to establish the planning baseline. After CI is implemented, PRs must run the test suites. A green badge without a linked successful run is insufficient.

## Definition of done

- Scope and acceptance conditions are satisfied.
- Relevant unit, integration and frontend tests ran and passed; browser evidence exists for changed user journeys.
- Evidence identifies the tested commit and environment.
- Spec/API changes are reflected in docs.
- No company data, credentials or generated database is committed.
- Remaining gaps are explicitly documented; the issue stays open if a required condition is unverified.

## Failure handling and local continuity

Distinguish product defects, test/environment failures and evaluator mistakes. Preserve the failing result before repair. Re-run the failing scenario and affected regressions. Do not silently convert an infrastructure error into a product pass.

Keep task_state.md and pitfall_log.md locally. Public sanitized evidence belongs in docs/evidence/. At session end, record progress, active issue, pending decisions and next command/task. Read these local files at the next session.
