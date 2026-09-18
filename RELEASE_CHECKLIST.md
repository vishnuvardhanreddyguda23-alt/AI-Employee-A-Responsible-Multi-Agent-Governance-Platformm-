# Final Release Checklist

- [x] Phase 1–5 modules retained; Phase 6 focuses on stability/portability.
- [x] Release tree excludes `.env`, `.git`, virtual environments, node_modules, caches, pyc files, logs and local DB files.
- [x] Docker Compose includes Postgres/pgvector, Redis, backend, frontend, Prometheus and Grafana.
- [x] Fresh migrations are run automatically.
- [x] Idempotent demo seeding is available.
- [x] Demo mode and environment template documented.
- [x] Documentation and Mermaid architecture diagrams included.
- [x] Demo credentials are explicitly non-production.
- [x] Security limitations documented.
- [ ] Production secrets, TLS, ingress, backups and external identity provider must be configured by the deployment owner.
- [ ] Full integration test execution requires Docker services and the project dependencies.
