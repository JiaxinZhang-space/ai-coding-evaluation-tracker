# Agent roles and handoff

The project uses explicit sequential roles. One assistant may perform multiple roles in separate steps; this is not a claim that independent agents ran. Parallel delegation is not required for the MVP.

| Role | Responsibility | Output and handoff |
| --- | --- | --- |
| Product planner | Scope, domain rules, acceptance IDs and issue dependencies | Ready issue and specification to implementer |
| Implementer | Read assigned issue, change only its scope, add meaningful tests | Commit, test commands/results and unresolved questions to verifier |
| Verifier | Check acceptance against actual behavior, challenge evidence and regressions | VERIFIED/FAILED/BLOCKED verdict with evidence to implementer or release role |
| Release operator | Compose, CI, deployment, rollback, final reviewer package | Run URLs, deployment smoke and rollback evidence |
| Human owner | Product scope decisions, paid hosting/credentials, course submission and peer reviews | Approval or correction; official completion receipts |

Workflow: planner prepares issue -> implementer posts start record -> implementer runs tests -> verifier checks acceptance -> failures return to implementer -> release operator checks integration -> issue closes with evidence. When one assistant fills both implementer and verifier roles, label the review as self-review.

Before handoff include issue URL, commit SHA, files changed, acceptance IDs, executed commands, failing or unverified items and the exact next action. Never manufacture role messages or independent review.

Phase 1 actual execution: Codex performed product planning, architecture design and documentation self-review. No application implementation or runtime verification occurred.
