# Public implementation backlog

[GitHub Issues](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues) is the public tracker. Task numbers below were created on 2026-10-30. P01 is the current documentation task; P02-P11 are planned and unverified.

| Task | Scope and acceptance | Dependencies |
| --- | --- | --- |
| [P01 Define product and verification plan](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/1) | English README, spec, architecture, AC01-AC24, rubric mapping, AGENTS and role/process docs exist; public backlog has acceptance criteria; documentation audit and initial Git commit are linked. | None |
| [P02 Build persistent task and criterion API](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/2) | AC01-AC05 and AC12 pass with real temporary SQLite databases; parameter validation and transaction failures preserve data; attach Pytest evidence. | P01; user starts implementation |
| [P03 Implement evaluations and immutable Debug history](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/3) | AC06-AC11 pass; unknown is distinct from zero; finalized history cannot change; cross-task references and partial writes are rejected; attach unit and integration evidence. | P02 |
| [P04 Build the evaluator UI](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/4) | AC02, AC03, AC08-AC11, AC14-AC15 pass through UI; include frontend tests and actual browser evidence at both viewports. | P03 |
| [P05 Package Compose and unified verification](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/5) | AC13, AC16 and AC17 pass from a clean clone; no manual seed/migration; verify runner fails on any suite failure and uses isolated data. | P04 |
| [P06 Automate PR tests and structured request logs](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/6) | AC18 and AC21 pass; link actual PR run; logs omit sentinel credentials and payloads; no deployment claimed. | P05 |
| [P07 Deploy demo and prove rollback](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/7) | AC19-AC20 pass; show actual URL or deployed demo video, revision and image digest; read-only enforced in API; record database survival after rollback. | P06; hosting decision |
| [P08 Add automatic dev and manual production deployment](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/8) | Successful main CI deploys dev; manual production promotion deploys the same tested digest; failed smoke prevents promotion; link actual workflow runs. | P07 |
| [P09 Correlate telemetry and trigger user-impact alert](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/9) | AC22-AC23 pass; stored logs, metrics and traces are correlated in UI; controlled journey failure triggers and resolves alert; no sensitive payloads. | P07 |
| [P10 Demonstrate alert-driven responder agent](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/10) | AC24 passes with actual agent tool/action evidence, recovery verification and bounded failed-repair escalation; no production authority. | P09 |
| [P11 Assemble submission and complete peer reviews](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/issues/11) | All selected baseline criteria have fresh evidence; score is evidence-based; clean-clone walkthrough complete; human submits and completes three peer reviews; certificate status verified on platform. | P07; optional P08-P10 only if selected |

## Delivery order

P01 is the Phase 1 gate. Implementation starts only after the user's next instruction: P02 -> P03 -> P04 -> P05 -> P06 -> P07. P11 prepares the final evidence and platform checklist. P08-P10 are optional score extensions and are not prerequisites for the planned 18-point baseline.

Rough effort estimates, not measured time: P01 2 hours; P02-P03 5 hours; P04 4 hours; P05-P06 4 hours; P07 and P11 3 hours. Total baseline estimate: 18 hours, with a separate 4-8 hour contingency. Optional operations/agent extensions may add 6-12 hours and should not displace verification.

## Agent tracker evidence

P01 includes a dated execution record linking the planning work to the tracker. Completion must attach a verified commit and document-audit evidence. Each implementation issue must similarly record a start comment and actual tests. Planned acceptance text alone does not close an issue.
