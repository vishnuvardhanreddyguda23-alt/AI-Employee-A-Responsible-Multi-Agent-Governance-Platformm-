# Quick Start

## Requirements
1. Install Docker Desktop.
2. Extract or clone this repository.
3. From the project root, copy `.env.example` to `.env`.
4. Keep `DEMO_MODE=true` for an API-key-free demonstration.
5. Run `docker compose up --build`.
6. Seed remaining governance data with `docker compose exec backend python -m scripts.seed_governance`.
7. Open http://localhost:5173.

## Demo accounts
These are intentionally non-production demo credentials:

| Role | Email | Password |
|---|---|---|
| Super Admin | admin@demo.local | DemoAdmin123! |
| Governance Admin | governance@demo.local | DemoGovernance123! |
| Department Manager | manager@demo.local | DemoManager123! |
| Viewer | viewer@demo.local | DemoViewer123! |
| Agent | agent@demo.local | DemoAgent123! |

Change/remove these accounts before any non-demo deployment.

## Optional services
- Backend/OpenAPI: http://localhost:8000/docs
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000

Real external LLM execution is optional. The included Phase 2/4/5 demonstrations use deterministic/offline adapters and seeded data; no paid API key is required for the basic UI demonstration.
