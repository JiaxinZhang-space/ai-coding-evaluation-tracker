# Product specification

## User and problem

A solo evaluator needs to answer: what was tested, against which rules, with what evidence, and what changed after a fix? The system stores manually entered observations. It does not judge models, execute submitted code or call model providers.

## Minimum product

1. Task list with title, description, lifecycle state and model evaluation count. Create/edit a task; archive it without deleting history.
2. Task detail with ordered criteria and their maximum integer points. Add/edit/delete criteria only before the first evaluation.
3. Evaluation form with model label, model version, result per criterion and evidence reference.
4. Evaluation detail with coverage, provisional/final total, immutable finalized records and a chronological Debug history.
5. Debug retest creation from an earlier finalized evaluation, with issue description and claimed fix. The new round starts with every criterion UNVERIFIED.

All UI and reviewer documentation will be English. Seed examples: fictional models MockCoder-A and MockCoder-B, tasks such as a toy CSV validator, and clearly labeled synthetic evidence references. Seed data is never product verification evidence.

## Domain rules

- Task title: 1-120 trimmed characters; description: 1-2000. States: DRAFT, ACTIVE, ARCHIVED. Allowed transitions and HTTP outcomes are specified below.
- Criterion title: 1-120; description: 1-2000; max_points: integer 1-10. DRAFT supports 0-20 criteria; ACTIVE requires 1-20 valid criteria. Ordering is explicit and stable by ID.
- Model label and version: each 1-80 characters; both required. Labels are plain strings rather than a separate model catalog.
- Evaluation states: DRAFT and FINALIZED. Criteria are locked at task level as soon as any evaluation exists. Evaluations reference stable criterion IDs.
- Each result has verification UNVERIFIED or VERIFIED, points (nullable integer), evidence_ref (plain text up to 500), and notes (up to 2000).
- UNVERIFIED requires null points. VERIFIED requires points within 0..max_points and a nonblank evidence_ref. Here VERIFIED means an evaluator has recorded an observation; the application cannot independently authenticate its truth.
- Any earned point value including zero is a valid verified observation. Passing/failing a model is not inferred from a partial score.
- Total maximum = sum of criterion maxima. Verified subtotal = sum of verified points. Coverage = verified criterion count / total criterion count. If coverage is incomplete, the total is labeled PROVISIONAL; unknown scores are not displayed as zero.
- Finalization requires all criteria VERIFIED and valid evidence references. It is atomic. Finalized results cannot be edited or deleted.
- A Debug round requires a finalized parent evaluation in the same task, a nonempty issue description and fix summary. Its results start UNVERIFIED, without copied scores/evidence. The earlier round remains unchanged.
- Debug differences are calculated per stable criterion ID and shown only when both observations are VERIFIED. Show a signed point delta without asserting causality.
- No hard deletion of tasks/evaluations in the MVP. DRAFT and ACTIVE task titles/descriptions remain editable until archive; criterion changes lock independently.

## Task lifecycle and mutation contract

These rules apply to existing resources with otherwise valid payloads in local write-enabled mode. Invalid fields return 422; unknown resources return 404. Every rejected mutation leaves database rows, counts, ordering and timestamps unchanged, including any other fields supplied in the same request.

| Operation and precondition | HTTP result | Stored outcome |
| --- | --- | --- |
| DRAFT -> ACTIVE with 1-20 valid criteria | 200 | State becomes ACTIVE |
| DRAFT -> ACTIVE with no criteria | 409 | No changes |
| ACTIVE -> DRAFT | 409 | No changes |
| DRAFT or ACTIVE -> ARCHIVED | 200 | State becomes ARCHIVED; history retained |
| Any mutation of ARCHIVED, including metadata edits or reactivation | 409 | No changes; reads remain available |
| Edit title/description of DRAFT or ACTIVE | 200 | Valid metadata saved; state unchanged |
| Add a valid criterion to an unlocked DRAFT or ACTIVE task with fewer than 20 criteria | 201 | One criterion added |
| Add a 21st criterion to an otherwise unlocked task | 422, code CRITERION_LIMIT_EXCEEDED | No changes; input error identifies the 20-criterion limit |
| Edit an unlocked criterion with valid fields | 200 | Criterion saved |
| Delete an unlocked criterion, except the last criterion of ACTIVE | 204 | Criterion removed |
| Delete the last criterion of ACTIVE | 409 | No changes |
| Create an Evaluation for DRAFT or ARCHIVED | 409 | No evaluation or result rows created |
| Create the first Evaluation for ACTIVE | 201 | Evaluation and results created atomically; task criteria become locked |
| Add/edit/delete/reorder criteria after any Evaluation exists | 409 | No changes, including scores and history |

DRAFT permits zero criteria; no task may exceed 20. ACTIVE must retain at least one valid criterion. ARCHIVED has no outgoing transition. Criterion locking does not prevent editing the title/description of an ACTIVE task. An Evaluation in DRAFT state still locks the task criteria; finalization is not required for the lock.

The Evaluation-creation guard and criterion lock after real Evaluation creation are scheduled for implementation and testing in P03. P02 implements task/criterion behavior before any Evaluation exists; it must not fabricate passing evidence for the later guard.

## Main journey

Create "Toy CSV Validator" with criteria Correctness (2), Error handling (2), Readability (1), then activate the task. Start MockCoder-A v1. Verify the first two with scores 2 and 0; coverage is 2/3 and provisional subtotal is 2/5. Finalization is rejected until Readability has evidence. Verify it with 1 and finalize at 3/5. Create a Debug round for v2; no scores are inherited. Record 2, 2, 1 and finalize at 5/5. The timeline shows a +2 change in Error handling and preserves v1 at 3/5.

## UI behavior

Forms have labels, keyboard focus, inline validation and disabled duplicate submission while saving. Every data view has loading, empty and API-error states. Errors retain entered values and offer retry. A successful save is shown only after the API confirms it. The task list filters by lifecycle state. Usable at 390px and 1280px viewport widths; verify both later.

## Excluded scope

Authentication, organizations, file attachments, private datasets, model execution, automatic scoring, rich text, report export, dashboards, arbitrary rubric versions and collaborative live editing. Public demonstration is read-only; reviewers test mutation flows locally. These exclusions keep evidence preservation testable within a small project.
