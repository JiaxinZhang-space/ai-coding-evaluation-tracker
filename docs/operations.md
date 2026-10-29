# Deployment and operations plan

## Base release

Plan one small Linux host running the same Compose app with durable disk and HTTPS termination. Hosting provider and budget remain undecided until the release task; no hosting purchase is authorized by this plan. A read-only public demo reduces abuse while local reviewers can test all write flows.

Build versioned images tagged by commit SHA. Set database path and demo mode through environment variables; keep secrets outside Git. Apply migrations on startup, back up the SQLite database consistently before migrations, then run health and read-journey smoke checks.

Record the actual URL, image digest, source revision, deployment date and smoke output. If no live URL is available, produce a clear recording of the real deployed environment and its revision, as allowed by the rubric; a localhost screenshot alone is not deployment evidence.

## Rollback

Retain the preceding image digest and a consistent pre-migration database backup. For a backward-compatible schema, deploy the preceding image and rerun health plus the task-detail journey. For an incompatible schema, stop writes and restore the backup before deploying the older image; record potential loss of writes since backup. Drill this in dev with synthetic data before using it for a release. Do not claim rollback works without AC20 evidence.

## CI base and optional CD

Base: pull_request and push-to-main workflows run the single verify interface and upload test artifacts. Do not expose deployment secrets to untrusted PRs. Optional: after a successful main run, publish SHA images and deploy dev; production promotion uses an explicit manual workflow and the tested digest. Serialize deployments and stop promotion on failed health checks. No branch protection is claimed unless actually configured.

## Base structured logs

Log timestamp, severity, request ID, route template, status and duration. Exclude request/response bodies, evidence text, model notes, authentication headers and secrets. Test sanitization with sentinel strings. Request IDs alone earn no claim of full distributed tracing.

## Optional observability and user impact alert

For AC22, use OpenTelemetry instrumentation on task-detail reads and evaluation writes, with an off-the-shelf collector and a UI that stores logs, traces and metrics. Final backend selection is deferred to avoid adding infrastructure before the core works. Include trace IDs in structured logs and trace exemplars/correlation links for metrics. Prove a single request can be followed across the UI.

For AC23, run a synthetic user-journey probe every 30 seconds against a seeded task-detail read. Alert after three consecutive failures and resolve after two successes. Force an API failure only in a controlled dev environment; record failure time, alert delivery, restoration and resolution. This measures a broken journey, not CPU usage.

## Optional responder

An actual agent receives the alert, reads allowlisted health/log evidence, selects an allowed recovery action such as rolling back the dev image, performs it and reruns the journey. Limit it to a disposable dev environment, one repair attempt per incident and explicit escalation on failure. Never grant arbitrary database deletion, production access or shell commands from alert text. Record tool calls and before/after evidence. No model/provider account or automated responder is implemented in Phase 1.
