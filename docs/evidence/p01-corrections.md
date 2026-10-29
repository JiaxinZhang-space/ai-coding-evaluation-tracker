# P01 targeted correction verification

Date: 2026-10-30 (Asia/Shanghai).
Role: specification maintainer and documentation self-reviewer.
Baseline: acc6daf1beea1cb2e5ac19b2fea03f25d7295f03.
Scope: documentation and tracker corrections only. No business implementation, application tests, database migration, CI or deployment was performed.

## Before and after

| Finding | Before | Corrected result |
| --- | --- | --- |
| F1 | P02/P03 referenced complete acceptance conditions containing later UI checks | Original AC01-AC24 retained; API/UI/Evaluation subconditions have explicit owners. P02 owns six backend conditions, P03 eight, P04 nine. Parent ACs require all subconditions before verification. |
| F2 | Lifecycle transitions and mutation boundaries were underspecified | Matching Spec/API tables define HTTP results, criterion limits, archive immutability, metadata updates and post-Evaluation locking. Rejected mutations require unchanged database state. |
| F4 | Startup required untracked local files without a fallback | Missing files trigger context recovery from public Issues, Backlog and Evidence Ledger; local files may then be created without blocking work. |
| F3 correction | Previous review incorrectly claimed criterion description was missing | Finding withdrawn. Original commit and GitHub blob both contain description; the criteria schema row is unchanged. |
| Runtime prerequisite | Audit named Windows PowerShell | PowerShell 7 (pwsh), rechecked on 7.6.5; Windows PowerShell 5.1 is explicitly unsupported. |
| Backlog status | P01 shown as the current task | P01 completed; P02 next, awaiting explicit user confirmation. |

See the [phase ownership and exact scenarios](../acceptance.md) and [HTTP contract](../architecture.md). The process definition of done now applies to the issue-owned layer, so backend completion does not require future frontend work.

## F3 evidence and correction

The original [architecture at acc6daf, line 37](https://github.com/JiaxinZhang-space/ai-coding-evaluation-tracker/blob/acc6daf/docs/architecture.md#L37) contains:

`criteria: id PK, task_id FK, title, description, max_points CHECK 1..10, position; unique task_id + position`

The original local Git blob and GitHub contents API returned the same blob ID: `0e25ac5cba1c287d794c00afc0a5129b7fb1547a`. The current criteria row was compared directly against that original row and is identical. No schema-field repair was made. The prior F3 review conclusion was incorrect.

Reproduction commands:

```powershell
git show acc6daf:docs/architecture.md | Select-String '^\| criteria \|'
git rev-parse acc6daf:docs/architecture.md
gh api 'repos/JiaxinZhang-space/ai-coding-evaluation-tracker/contents/docs/architecture.md?ref=acc6daf' --jq .sha
```

## Executed checks

- Environment: PowerShell 7.6.5.
- `pwsh -NoProfile -File .local/check-p01-corrections.ps1`: documentation consistency helper, exit 0. It compared condition sets across Acceptance, Backlog and freshly read GitHub Issue bodies; verified identical lifecycle tables, unchanged original criteria row, unchanged AC13-AC24, local-file fallback and Markdown-only changes. The helper is local tooling, not an application test.
- P02 exact owned set: AC01, AC02-API, AC03-API, AC04, AC05-API, AC12-TASK.
- P03 exact owned set: AC05-EVAL, AC06, AC07, AC08-API, AC09-API, AC10-API, AC11-API, AC12-EVAL.
- P04 exact owned set: AC02-UI, AC03-UI, AC08-UI, AC09-UI, AC10-UI, AC11-UI, AC12-UI, AC14, AC15.
- The published Phase 1 document-audit script was rerun: original parent IDs remain 24; rubric rows remain 12 and planned total remains 18; relative links resolve; no runtime source files.
- `git diff --check`: exit 0.
- Updated Issues 2, 3 and 4 were read back with `gh issue view --json number,body,state,url`; each body matched the prepared text exactly, remained OPEN/PLANNED and had no checked acceptance boxes.
- Anonymous HTTP GETs to Issues 2-4 and the original architecture evidence URL returned 200.
- The first general audit rerun rejected repeated parent-ID references in the new ownership table. The checker was narrowed to the original parent table and rerun successfully; this was a documentation-checker issue, not an application test result.

The final commit SHA, final link count and push verification are recorded in the new P01 completion comment. Historical comments and historical test outcomes were not fabricated or rewritten.

## Readiness and remaining limits

Documentation readiness for an independent P02: PASS. P02 requires only task/criterion APIs, migrations/seeding and local Pytest with temporary SQLite; no UI, Evaluation implementation or P05 container verifier is needed for its completion.

All runtime parent and subconditions remain UNVERIFIED. P02 has not started. Post-Evaluation criterion locking, Evaluation creation guards, Debug and scoring verification belong to P03; UI/browser verification belongs to P04.

F5 deployment evidence alternatives remain unresolved until before P07, as explicitly requested. Course-platform eligibility/deadline confirmation and the existing submission checklist are unchanged. Do not interpret documentation readiness as certificate eligibility.

Stop after this correction commit and wait for user confirmation before P02.
