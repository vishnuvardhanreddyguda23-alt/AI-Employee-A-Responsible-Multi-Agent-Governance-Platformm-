# Phase 5 — Enterprise Observability, Evaluation, Audit and Deployment

Implemented on top of Phase 4; previous governance modules remain intact.

## Enterprise audit
`/api/enterprise/audit` exposes timestamp, actor, agent, action, resource, result, risk, request ID and metadata. Audit rows use a previous-hash/integrity-hash chain; `/api/enterprise/audit/integrity` verifies the chain. The system never exposes model chain-of-thought.

## Decision traces
`/api/enterprise/decision-traces` records request, workflow state, retrieved sources, tools, policy checks, risk/approval result, final output and evaluation result.

## Evaluation
Demo dataset is seeded by `python -m scripts.seed_phase5`. Run `POST /api/enterprise/evaluations/run` and inspect reports. Metrics cover task success, grounding, correctness, format compliance, latency, tool accuracy and safety compliance.

## RAG
PostgreSQL remains the knowledge store. The migration enables `pgvector` when available and the demo keeps a portable JSON embedding fallback. Ingest via `/api/enterprise/rag/ingest`; semantic retrieval via `/api/enterprise/rag/search`. Replace `pseudo_embedding()` with the production embedding provider without changing the API contract.

## Observability
`/metrics` exposes Prometheus metrics for request count and latency. Docker Compose includes Prometheus and Grafana. OpenTelemetry dependencies are included for future exporter wiring without forcing an external collector in local development.

## Kubernetes
`k8s/` contains backend, frontend, Postgres (pgvector image), Redis, config and secret examples. Kubernetes is optional; Docker Compose remains the easiest local deployment.

## Security hardening
PII masking is applied to audit reasons and retrieved demo content. Backend RBAC continues to gate mutation endpoints. CORS remains environment-configured. Never commit real `.env`, JWT secret, database passwords or provider keys. Production should put secrets in a secret manager and use managed Postgres/Redis.
