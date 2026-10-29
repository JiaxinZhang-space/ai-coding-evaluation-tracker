# Acceptance conditions and test plan

Every row starts UNVERIFIED. The acceptance IDs below are stable and must appear in issues and evidence records. Tests are planned, not executed.

| ID | Given and action | Required observable result | Test layer |
| --- | --- | --- | --- |
| AC01 | Fresh empty database; run startup twice | Same synthetic IDs/counts, no duplicates; health succeeds | Pytest integration |
| AC02 | Create a valid task and reload | Same server ID and fields returned and visible | API integration + browser |
| AC03 | Submit blank title or overlong description | 422 and no new row; UI explains errors and retains input | API + frontend |
| AC04 | Exercise task activation, forbidden downgrade and writable metadata | Exact lifecycle HTTP results; rejected writes leave database unchanged | P02 integration |
| AC05 | Exercise criterion validation, count limits, last-criterion guard and Evaluation lock | Before Evaluation: AC05-API; after Evaluation: AC05-EVAL; rejection leaves database unchanged | P02 + P03 integration |
| AC06 | Start Evaluation for ACTIVE, DRAFT and ARCHIVED tasks | ACTIVE: 201 with unknown results; DRAFT/ARCHIVED: 409 with no inserts | P03 integration |
| AC07 | Save verified result without evidence, fractional/out-of-range points or another task's criterion | 422/404 as appropriate; no partial write | Unit + integration |
| AC08 | Scores 2,0,unknown against maxima 2,2,1 | Coverage 2/3; subtotal 2/5 PROVISIONAL; finalization rejected | Unit + UI |
| AC09 | Complete scores 2,0,1 with evidence; finalize | FINALIZED total 3/5; further result edits return 409 | Unit + integration + browser |
| AC10 | Create Debug round from finalized v1 | Correct parent and descriptions; all new scores unknown; v1 unchanged | Integration + browser |
| AC11 | Finalize v2 at 2,2,1 and open timeline | v1 3/5, v2 5/5; only error handling delta +2 | Unit + browser |
| AC12 | Archive task then attempt task/criterion and Evaluation mutations; inspect UI | History remains readable; writes return 409; archived UI is read-only | P02 + P03 integration + P04 browser |
| AC13 | Save task; docker compose down; start again without deleting volume | Exact task ID, observations and history survive | Container integration |
| AC14 | Trigger API error during form save | No false success; values preserved; retry succeeds | Frontend + browser |
| AC15 | Open empty/list/detail/form views at 390px and 1280px; use keyboard | No lost controls/horizontal clipping; labels and focus usable | Browser + recorded manual QA |
| AC16 | Start from clean clone using planned startup command | UI and API ready without manual migration/seed steps | Container smoke |
| AC17 | Run the single verify command | Backend unit/integration, frontend and E2E suites execute; aggregate nonzero on failure | Test runner |
| AC18 | Push a PR changing app code | GitHub Actions automatically runs required suites and records results | Remote CI |
| AC19 | Open public deployment and call mutation endpoint directly | Read journey works; write returns 403; banner visible | Deployed smoke |
| AC20 | Deploy bad image in dev, then follow rollback runbook | Prior image serves healthy journey; database record survives | Deployment drill |
| AC21 | Inspect structured logs after a request | Timestamp, route template, status, duration, request ID; no body/credentials | Integration + log inspection |
| AC22 | Optional: run instrumented journey | Stored metrics, traces and logs correlate through trace ID in UI | Operations integration |
| AC23 | Optional: induce journey failures in dev and recover | Alert fires after documented window; notification and resolution recorded | Alert drill |
| AC24 | Optional: alert invokes responder | Agent diagnoses allowed fault, fixes it and verifies recovery; action log retained | Agent drill |

## Phase ownership and completion

The original AC01-AC24 IDs remain the parent requirements. Subconditions below split execution responsibility without weakening the parent. Every runtime parent and subcondition is currently UNVERIFIED. A parent becomes VERIFIED only when all its listed subconditions have passing evidence at the relevant revision; completing P02 never marks unimplemented Evaluation or UI behavior as passed.

| Condition | Owner | Required verification |
| --- | --- | --- |
| AC01 | P02 | Run initial schema/seed startup twice against a temporary SQLite file; verify stable IDs/counts and health. |
| AC02-API | P02 | POST a valid task returns 201; subsequent GET returns the same ID, title, description and DRAFT state. |
| AC02-UI | P04 | Create a task through the UI and reload the page; the saved fields remain visible. |
| AC03-API | P02 | Blank title or overlong description returns 422 with unchanged database state. |
| AC03-UI | P04 | Invalid task input shows field errors, retains entered values and never shows false success. |
| AC04 | P02 | Test DRAFT -> ACTIVE with zero criteria (409) and valid criteria (200); ACTIVE -> DRAFT (409); valid title/description edits in both writable states (200). Assert atomic failure even for combined metadata/state PATCH. |
| AC05-API | P02 | Before any Evaluation exists, add/edit/delete criteria; enforce field validation (422), the 20-criterion cap in DRAFT and ACTIVE (21st create: 422 + CRITERION_LIMIT_EXCEEDED), and ACTIVE last-criterion deletion (409). DRAFT may delete its final criterion. Verify description round trips and no database changes after rejection. |
| AC05-EVAL | P03 | Create a real Evaluation through the API, then reject all criterion additions, edits, deletions and reorder changes with 409. Verify unchanged criteria, results and history. No fixture-only lock proof. |
| AC06 | P03 | ACTIVE Evaluation creation returns 201 with one UNVERIFIED/null-points result per criterion. DRAFT and ARCHIVED creation returns 409 with no inserted rows. |
| AC07 | P03 | Invalid points or missing evidence returns 422; a criterion outside the Evaluation task returns 404. Every rejection leaves all business rows unchanged. |
| AC08-API | P03 | Unit and SQLite/API tests: scores 2,0,unknown against maxima 2,2,1 produce coverage 2/3 and provisional subtotal 2/5; finalization returns 409 with unchanged data. |
| AC08-UI | P04 | Render the same partial result as PROVISIONAL with unknown distinct from zero; attempting finalization shows rejection without losing input. |
| AC09-API | P03 | Finalize verified scores 2,0,1 with evidence: 200, FINALIZED, total 3/5. Further result edits return 409 without changing the Evaluation or results. |
| AC09-UI | P04 | Finalize through the UI, reload, and see FINALIZED 3/5; subsequent editing cannot change the finalized record. |
| AC10-API | P03 | Create Debug round through API: parent must be finalized and in the same task; new result rows are UNVERIFIED with null points and no copied evidence. Invalid relationships to an existing parent return 409; missing required Debug fields return 422; failures leave all rows unchanged. |
| AC10-UI | P04 | Create the Debug round using UI inputs, verify its parent/descriptions and fresh unknown results; the original round remains readable. |
| AC11-API | P03 | Unit and SQLite/API tests finalize v2 at 2,2,1; comparison returns v1 3/5, v2 5/5 and error-handling delta +2 by stable criterion ID. Do not compute deltas for unverified observations. |
| AC11-UI | P04 | Timeline displays both rounds and the +2 error-handling change while preserving v1; unverified comparisons do not show a numeric delta. |
| AC12-TASK | P02 | Archive DRAFT and ACTIVE tasks (200). Reads remain 200. Task metadata edits, state changes/reactivation and criterion POST/PATCH/DELETE under the archived task return 409 with unchanged data. |
| AC12-EVAL | P03 | Archive a task with actual Evaluations/results; Evaluation creation, result writes and finalization return 409. Reads preserve the full history. Rejected writes change no rows. |
| AC12-UI | P04 | Archive through the UI, reload and inspect retained task/history; archived editing and reactivation are unavailable and API rejections never show success. |

AC14 and AC15 belong to P04. AC13 and AC16-AC24 retain their existing owners in the backlog. AC02, AC03, AC05 and AC08-AC12 have multiple phase-owned subconditions; AC04, AC06 and AC07 are backend-only. P03 owns scoring unit tests and real SQLite/API Evaluation and Debug integration; P04 owns frontend tests and real browser journeys.

P02 is independently completable with only task/criterion APIs, migrations, idempotent synthetic task/criterion seeds and temporary SQLite tests. It does not implement Evaluation creation, scoring, Debug or the UI. AC05-EVAL and AC12-EVAL deliberately remain UNVERIFIED until P03; no fake passing lock or history evidence is acceptable. P03 adds the Evaluation persistence/API and the post-Evaluation criterion guard.

The phase test commands may use local Pytest/frontend tooling before P05 exists. The unified container verification command is a P05 deliverable, not a prerequisite for closing P02 or P03.

## Rejected mutation verification

For every specified 409, 422 or 404 mutation failure, take a logical snapshot of all existing business tables before the request and compare after it, including IDs, counts, values, criterion ordering and timestamps. Assert the exact HTTP status and error code where specified. No partial metadata update, inserted Evaluation/result, deletion or score change is permitted. Use real temporary on-disk SQLite databases. Test one rejection cause at a time with otherwise valid input; also test a combined valid-metadata/invalid-state PATCH to prove atomic failure.

Lifecycle outcome authority is the matching table in the [product specification](product-spec.md) and [API contract](architecture.md). Archived-task checks here use local write-enabled mode; hosted read-only mode is independently verified by AC19.

## Testing approach

Use Pytest for score computation, validation and SQLite/API integration through HTTP requests. Integration tests exercise real SQL with temporary on-disk databases, including atomic failure and restart behavior. Pure arithmetic unit tests alone do not prove persistence.

Use Vitest and React Testing Library for validation, loading/error rendering and partial-score labels. Use Playwright against running containers for AC02 and the complete AC08-AC11 journey. The browser test must create mock data through UI inputs, not prepopulate a passing outcome through direct API calls.

The verify service may orchestrate isolated API/web subprocesses internally; the chosen implementation must ensure a clean database per run and propagate every suite's exit code. Cache downloads if useful, never cache results as fresh evidence.

## Evidence record

For each run record: acceptance IDs; UTC timestamp and local date; source commit SHA; role; environment and tool versions; exact command; exit code; expected and observed behavior; artifact path or Actions URL; verdict; limitations. For manual UI checks include browser, viewport, steps and screenshots. Include failing runs and their fixes; do not overwrite historical evidence.

A feature is VERIFIED only when its acceptance conditions pass against the relevant revision. Existing evidence can be retained for unaffected behavior with an explicit change-impact rationale. A full regression is required for the release candidate.
