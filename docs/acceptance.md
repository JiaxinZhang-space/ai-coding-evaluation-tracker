# Acceptance conditions and test plan

Every row starts UNVERIFIED. The acceptance IDs below are stable and must appear in issues and evidence records. Tests are planned, not executed.

| ID | Given and action | Required observable result | Test layer |
| --- | --- | --- | --- |
| AC01 | Fresh empty database; run startup twice | Same synthetic IDs/counts, no duplicates; health succeeds | Pytest integration |
| AC02 | Create a valid task and reload | Same server ID and fields returned and visible | API integration + browser |
| AC03 | Submit blank title or overlong description | 422 and no new row; UI explains errors and retains input | API + frontend |
| AC04 | Activate a task with zero criteria; then add valid criteria | First returns 409; activation succeeds only after criteria exist | Integration |
| AC05 | Edit/delete criterion before and after evaluation creation | Allowed before; 409 after; stored rule unchanged | Integration |
| AC06 | Start evaluation for active task | One result per criterion; all UNVERIFIED with null points | Integration |
| AC07 | Save verified result without evidence, fractional/out-of-range points or another task's criterion | 422/404 as appropriate; no partial write | Unit + integration |
| AC08 | Scores 2,0,unknown against maxima 2,2,1 | Coverage 2/3; subtotal 2/5 PROVISIONAL; finalization rejected | Unit + UI |
| AC09 | Complete scores 2,0,1 with evidence; finalize | FINALIZED total 3/5; further result edits return 409 | Unit + integration |
| AC10 | Create Debug round from finalized v1 | Correct parent and descriptions; all new scores unknown; v1 unchanged | Integration + browser |
| AC11 | Finalize v2 at 2,2,1 and open timeline | v1 3/5, v2 5/5; only error handling delta +2 | Unit + browser |
| AC12 | Archive task then attempt all mutation routes | Task still readable; all writes return 409 | Integration |
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

## Testing approach

Use Pytest for score computation, validation and SQLite/API integration through HTTP requests. Integration tests exercise real SQL with temporary on-disk databases, including atomic failure and restart behavior. Pure arithmetic unit tests alone do not prove persistence.

Use Vitest and React Testing Library for validation, loading/error rendering and partial-score labels. Use Playwright against running containers for AC02 and the complete AC08-AC11 journey. The browser test must create mock data through UI inputs, not prepopulate a passing outcome through direct API calls.

The verify service may orchestrate isolated API/web subprocesses internally; the chosen implementation must ensure a clean database per run and propagate every suite's exit code. Cache downloads if useful, never cache results as fresh evidence.

## Evidence record

For each run record: acceptance IDs; UTC timestamp and local date; source commit SHA; role; environment and tool versions; exact command; exit code; expected and observed behavior; artifact path or Actions URL; verdict; limitations. For manual UI checks include browser, viewport, steps and screenshots. Include failing runs and their fixes; do not overwrite historical evidence.

A feature is VERIFIED only when its acceptance conditions pass against the relevant revision. Existing evidence can be retained for unaffected behavior with an explicit change-impact rationale. A full regression is required for the release candidate.
