# AI Employee — Responsible Multi-Agent Governance Platform

An enterprise-style control plane for registering, executing, observing and governing AI employees with deterministic authorization, risk controls, human approval, auditability and responsible-AI evidence.

## Problem
AI agents can act quickly across business systems, so organizations need controls for identity, permissions, sensitive data, financial impact, security events, human approval, cost and accountability.

## Solution
The platform combines an AI workforce registry with LangGraph execution, deterministic governance, risk scoring, human approvals, audit/decision traces, evaluation, knowledge grounding, workflow observability and deployment tooling.

## Key Features
- JWT authentication and backend RBAC
- AI employee registry, lifecycle and tool grants
- Deterministic governance and configurable policies
- LOW/MEDIUM/HIGH/CRITICAL risk assessment with reasons
- Human approval center and emergency agent/system controls
- Security event monitoring and PII masking helpers
- Tamper-evident audit hash chain
- Evidence-based agent decision traces without private chain-of-thought
- Evaluation datasets and reports
- PostgreSQL + pgvector-ready knowledge base and grounded retrieval hooks
- LangGraph demo workflows with observable state transitions
- Token, cost, performance and health monitoring
- Prometheus, Grafana and OpenTelemetry instrumentation
- Docker Compose and Kubernetes-ready manifests
- Offline/demo mode with deterministic seeded data

## Technology Stack
React + TypeScript + Vite + Tailwind; FastAPI + Pydantic + SQLAlchemy + Alembic; PostgreSQL 16/pgvector; Redis; LangGraph; Prometheus; Grafana; OpenTelemetry; Docker; Kubernetes manifests.

## Quick Start
```bash
cp .env.example .env
docker compose up --build
```
Then seed governance data once if needed:
```bash
docker compose exec backend python -m scripts.seed_governance
```
Open http://localhost:5173. See [QUICKSTART.md](QUICKSTART.md) for demo credentials.

## Demo Workflow
See [DEMO.md](DEMO.md) for the 10–15 minute presentation path.

## Documentation
- [QUICKSTART.md](QUICKSTART.md)
- [ARCHITECTURE.md](ARCHITECTURE.md)
- [API.md](API.md)
- [DATABASE.md](DATABASE.md)
- [SECURITY.md](SECURITY.md)
- [RESPONSIBLE_AI.md](RESPONSIBLE_AI.md)
- [DEPLOYMENT.md](DEPLOYMENT.md)
- [DEMO.md](DEMO.md)
- [CONTRIBUTING.md](CONTRIBUTING.md)
- [RELEASE_CHECKLIST.md](RELEASE_CHECKLIST.md)

## Project Structure
```text
ai-employee-governance-platform/
├── backend/
├── frontend/
├── agents/
├── governance/
├── infrastructure/
├── tests/
├── scripts/
├── docs/
├── k8s/
├── monitoring/
├── .env.example
├── docker-compose.yml
├── README.md
├── QUICKSTART.md
└── LICENSE
```

## Responsible AI
The system does not rely on an LLM for authorization. High-impact actions can require human approval. Decision traces record evidence rather than hidden chain-of-thought. Outputs may be marked `UNVERIFIED`. The project does not claim perfect hallucination detection or complete security.

## Screenshots
Add deployment-specific screenshots here after running the application; no fake screenshots are shipped in the release.

## Testing
Backend tests live in `backend/tests`; frontend production build uses `npm run build`. A complete integration run requires the project dependencies and PostgreSQL/Redis services.

## Limitations
The included agent adapters and embeddings are designed for a portable demonstration. External LLM execution, production-grade embeddings, centralized rate limiting, TLS/ingress, backups and enterprise identity integration require deployment-specific configuration.

## Future Work
Production embedding providers, centralized distributed rate limiting, stronger SSO/SCIM integration, managed telemetry, richer evaluation datasets and organization-specific tool connectors.
