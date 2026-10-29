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

- Task title: 1-120 trimmed characters; description: 1-2000. States: DRAFT, ACTIVE, ARCHIVED. ACTIVE requires at least one criterion. ARCHIVED is read-only.
- Criterion title: 1-120; description: 1-2000; max_points: integer 1-10. A task supports 1-20 criteria when active. Ordering is explicit and stable by ID.
- Model label and version: each 1-80 characters; both required. Labels are plain strings rather than a separate model catalog.
- Evaluation states: DRAFT and FINALIZED. Criteria are locked at task level as soon as any evaluation exists. Evaluations reference stable criterion IDs.
- Each result has verification UNVERIFIED or VERIFIED, points (nullable integer), evidence_ref (plain text up to 500), and notes (up to 2000).
- UNVERIFIED requires null points. VERIFIED requires points within 0..max_points and a nonblank evidence_ref. Here VERIFIED means an evaluator has recorded an observation; the application cannot independently authenticate its truth.
- Any earned point value including zero is a valid verified observation. Passing/failing a model is not inferred from a partial score.
- Total maximum = sum of criterion maxima. Verified subtotal = sum of verified points. Coverage = verified criterion count / total criterion count. If coverage is incomplete, the total is labeled PROVISIONAL; unknown scores are not displayed as zero.
- Finalization requires all criteria VERIFIED and valid evidence references. It is atomic. Finalized results cannot be edited or deleted.
- A Debug round requires a finalized parent evaluation in the same task, a nonempty issue description and fix summary. Its results start UNVERIFIED, without copied scores/evidence. The earlier round remains unchanged.
- Debug differences are calculated per stable criterion ID and shown only when both observations are VERIFIED. Show a signed point delta without asserting causality.
- No hard deletion of tasks/evaluations in the MVP. Draft task metadata remains editable until archive; criterion changes lock independently.

## Main journey

Create "Toy CSV Validator" with criteria Correctness (2), Error handling (2), Readability (1). Start MockCoder-A v1. Verify the first two with scores 2 and 0; coverage is 2/3 and provisional subtotal is 2/5. Finalization is rejected until Readability has evidence. Verify it with 1 and finalize at 3/5. Create a Debug round for v2; no scores are inherited. Record 2, 2, 1 and finalize at 5/5. The timeline shows a +2 change in Error handling and preserves v1 at 3/5.

## UI behavior

Forms have labels, keyboard focus, inline validation and disabled duplicate submission while saving. Every data view has loading, empty and API-error states. Errors retain entered values and offer retry. A successful save is shown only after the API confirms it. The task list filters by lifecycle state. Usable at 390px and 1280px viewport widths; verify both later.

## Excluded scope

Authentication, organizations, file attachments, private datasets, model execution, automatic scoring, rich text, report export, dashboards, arbitrary rubric versions and collaborative live editing. Public demonstration is read-only; reviewers test mutation flows locally. These exclusions keep evidence preservation testable within a small project.
