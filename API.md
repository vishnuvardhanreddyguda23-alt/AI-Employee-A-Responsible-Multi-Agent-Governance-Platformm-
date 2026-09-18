# API

FastAPI publishes the authoritative interactive OpenAPI specification at `/docs` and `/openapi.json`.

Key groups include:
- `/api/auth/*` authentication
- `/api/agents/*` agent registry, lifecycle and execution
- `/api/governance/*` governance overview and controls
- `/api/policies/*` policy administration
- `/api/approvals/*` human approval
- `/api/security/*` security events
- `/api/risk/*` risk assessments
- `/api/audit` audit records
- `/api/evaluations/*` evaluation datasets/reports
- `/api/workflows/*` observable workflow traces
- `/api/knowledge/*` knowledge ingestion/retrieval
- `/metrics` Prometheus metrics

Authentication uses a bearer JWT. Backend dependencies enforce RBAC; frontend visibility is not a security boundary.
