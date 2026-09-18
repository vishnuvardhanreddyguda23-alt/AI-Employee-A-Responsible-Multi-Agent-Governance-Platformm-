# Deployment

## Local / demo
```bash
cp .env.example .env
docker compose up --build
docker compose exec backend python -m scripts.seed_governance
```

## Development
Run Postgres/Redis with Compose and the backend/frontend in local development environments. Use the same environment variables as `.env.example`.

## Kubernetes
Manifests are in `k8s/` for backend, frontend, Postgres and Redis. They are a deployment starting point, not a claim of production-ready cluster operations. Review storage, ingress, TLS, secret management, resource limits, backups and network policies before production.

## Monitoring
Prometheus scrapes `/metrics`; Grafana is provisioned with a demo dashboard. OpenTelemetry FastAPI instrumentation is enabled when the installed instrumentation is available.
