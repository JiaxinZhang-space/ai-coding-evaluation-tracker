# Official rubric mapping

## Source and interpretation

Verified 2026-10-30 (Asia/Shanghai): [official project rubric at commit 526d9c12506c159e7a8f3cef30e5de1eeaec287b](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp/blob/526d9c12506c159e7a8f3cef30e5de1eeaec287b/project/README.md). That file was last changed on 2026-10-16. Recheck the moving main branch before submission.

The official total is 24 across 12 criteria; passing requires 10 and three peer reviews. The two attempts use the same project criteria. English documentation is required. Deadlines and workflow belong to the [course platform](https://courses.datatalks.club/ai-dev-tools-2026/). This page does not establish the user's certificate eligibility.

## Scoring ladder summary

The following concise ladder is a navigation aid; the linked official text controls grading.

| Criterion | Score ladder |
| --- | --- |
| Problem Description | 0 absent; 1 vague; 2 clear behavior |
| Product Specification | 0 absent; 1 spec only; 2 public accepted tasks and agent usage evidence |
| Agent Team Specification | 0 absent; 1 unclear roles; 2 responsibilities and collaboration |
| Context Management | 0 absent; 1 AGENTS; 2 modular references |
| Application | 0 missing layer; 1 disconnected; 2 persistent end-to-end, one command |
| Testing | 0 unrunnable; 1 one layer; 2 unit plus integration, one command |
| Containerization | 0 absent; 1 manual; 2 complete single-command containers |
| Deployment | 0 absent; 1 instructions; 2 demonstrated deployment |
| CI/CD | 0 absent; 1 PR tests; 2 automatic deployment; 3 dev automatic, production manual |
| Observability | 0 absent; 1 logs; 2 correlated logs/metrics/traces, stored UI, no sensitive data |
| Alerting on User Impact | 0 absent; 1 alert |
| On-Call Agent | 0 absent; 2 alert-driven repair |

## Implementation and evidence mapping

Targets are planned points, not awarded scores. Every runtime row remains UNVERIFIED in Phase 1.

| Criterion | Base target | Minimal planned delivery | Required reviewer evidence |
| --- | ---: | --- | --- |
| Problem Description | 2 | README with concrete evaluator problem and v1/v2 journey | README at submitted revision |
| Product Specification | 2 | Product spec and public GitHub Issues with AC IDs | P01 start/completion records, subsequent agent issue use, linked commits |
| Agent Team Specification | 2 | Sequential roles and explicit self-review/handoff | docs/team.md and actual role records |
| Context Management | 2 | Short AGENTS linking focused docs | Working links and agent startup records |
| Application | 2 | React + FastAPI + SQLite workflow | AC02, AC08-AC13 and AC16 logs/video |
| Testing | 2 | Pytest unit/integration plus frontend/E2E checks | AC17 command output, suite counts and exit codes |
| Containerization | 2 | Web and API containers; SQLite named volume | AC13/AC16 clean-clone and restart evidence |
| Deployment | 2 | Read-only hosted demo or clear recorded deployed demo | AC19 public URL or deployment recording, revision |
| CI/CD | 1 | PR-triggered GitHub Actions test workflow | AC18 real PR run URL |
| Observability | 1 | Structured request logs with sanitized metadata | AC21 real log inspection |
| Alerting on User Impact | 0 | Deferred until baseline is verified | AC23 only if implemented |
| On-Call Agent | 0 | Deferred until alerting works | AC24 only if implemented |
| Total | 18 | Planned baseline with eight points above the pass threshold | Peer reviewers award the actual score |

The base target exceeds the pass threshold by eight points. The plan deliberately retains deployment and frontend tests rather than relying on documentation alone. A deferred criterion contributes zero. Missing an assumed target must reduce the forecast; do not replace absent evidence with confidence.

## Optional six points

CI/CD can add two points with automatic dev deployment and manual production promotion. Full observability adds one point beyond logs. One user-impact alert adds one. A working alert-triggered responder adds two. Total maximum: 18 + 2 + 1 + 1 + 2 = 24.

The introductory course workflow includes telemetry, alert triggers and a responder. These are explicitly deferred in the minimum scoring plan, not described as completed. Implement them only after the core is reliable and if time permits. Do not equate a cron restart script with an agent.

## Current assessment

Documents can be inspected in Phase 1; public tracker usage must be verified after publication. No runtime points are claimed. No peer review result or certificate has been obtained through this work.
