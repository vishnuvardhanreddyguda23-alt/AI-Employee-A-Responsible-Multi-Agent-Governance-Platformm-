# Phase 5 implementation notes

This archive continues Phase 4; it does not replace the Phase 4 governance engine.

Added:
- tamper-evident audit hash chain and integrity verification
- enterprise audit/decision trace APIs
- evaluation datasets and report generation
- PostgreSQL/pgvector-ready knowledge base and semantic retrieval
- observable multi-agent LangGraph demo workflow
- Prometheus metrics + OpenTelemetry FastAPI instrumentation hook
- Prometheus/Grafana Docker Compose services
- Kubernetes manifests for backend/frontend/Postgres/Redis + secret example
- rate limiting and PII masking helpers
- Phase 5 enterprise observability frontend
- Phase 5 unit/workflow tests
- automatic Phase 5 demo seed in Docker Compose

Run locally:

```bash
docker compose up --build
```

For a non-Docker frontend:

```bash
cd frontend
npm install
npm run build
npm run dev
```

Backend migration and demo seed are run by Compose. The Phase 5 migration is `0005_enterprise`.
