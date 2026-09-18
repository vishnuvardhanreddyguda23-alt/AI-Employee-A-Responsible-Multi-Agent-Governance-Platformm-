# Phase 5 Quick Start

```bash
docker compose up --build
```

- Frontend: http://localhost:5173
- Backend API docs: http://localhost:8000/docs
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000

Database migration and Phase 5 demo seed run automatically in the backend container.

For Kubernetes, build `ai-employee-backend:phase5` and `ai-employee-frontend:phase5`, create the secret from `k8s/secrets.example.yaml`, then apply the manifests in `k8s/`.
