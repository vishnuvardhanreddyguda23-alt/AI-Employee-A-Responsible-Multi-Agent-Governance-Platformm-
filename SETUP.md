# SETUP.md

## Prerequisites

- Docker + Docker Compose v2 (`docker compose version`)
- For local (non-Docker) backend dev: Python 3.12+
- For local (non-Docker) frontend dev: Node.js 20+

## Option A — Docker Compose (recommended)

```bash
git clone <repo-url>
cd ai-employee-governance
cp .env.example .env
# Edit .env: set a real SECRET_KEY and POSTGRES_PASSWORD

docker compose up --build
```

This starts `postgres`, `redis`, `backend` (runs `alembic upgrade head`
then `uvicorn`), and `frontend`.

- Backend: http://localhost:8000 (docs at `/docs`)
- Frontend: http://localhost:5173
- Health check: `curl http://localhost:8000/health`

## Option B — Local backend, no Docker

```bash
cd backend
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements-dev.txt
cp .env.example .env               # point DATABASE_URL / REDIS_URL at local services
alembic upgrade head
uvicorn app.main:app --reload
```

Requires a locally running PostgreSQL and Redis matching the `.env`
values (or adjust `DATABASE_URL` / `REDIS_URL` accordingly).

## Option C — Local frontend, no Docker

```bash
cd frontend
npm install
cp .env.example .env               # set VITE_API_BASE_URL if backend isn't on :8000
npm run dev
```

## Running Tests

```bash
cd backend
source .venv/bin/activate
pytest -v
```

Backend tests run against an in-memory SQLite database via test
fixtures — no live Postgres/Redis required to run the test suite.

## Verifying Phase 1 Completion

```bash
curl http://localhost:8000/health
# {"api":"ok","database":"ok","redis":"ok","healthy":true}

curl -X POST http://localhost:8000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","full_name":"Admin","password":"ChangeMe123"}'

curl -X POST http://localhost:8000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","password":"ChangeMe123"}'
# copy access_token from the response

curl http://localhost:8000/api/auth/me -H "Authorization: Bearer <access_token>"
```

Then open http://localhost:5173/login in a browser and sign in with the
same credentials.

## Validation Status

This codebase has been verified locally:
- Python dependencies installed successfully in the virtual environment.
- Python 3.9 type-compatibility issues resolved (using `typing.Optional` instead of standard union `|` operator).
- `passlib` and `bcrypt` package conflict resolved by pinning `bcrypt==4.0.1`.
- Running the full backend test suite (`pytest`) results in 100% success (12/12 tests passed).


---

## Phase 2 Verification

```bash
# after alembic upgrade head:
cd backend
python -m scripts.seed_demo_data

curl http://localhost:8000/api/agents -H "Authorization: Bearer <token>"

curl -X POST http://localhost:8000/api/agents/<agent_id>/execute \
  -H "Authorization: Bearer <token>" -H "Content-Type: application/json" \
  -d '{"message": "I have a ticket issue"}'
# only works for the 3 seeded executable agents (execution_key set) once ACTIVE
```

Backend tests: `cd backend && pytest -v`. The agent-execution tests
(`tests/test_agent_execution.py`) are skipped automatically if
`langgraph` isn't installed, rather than failing the whole suite.

## Known Limitation of This Delivery (Phase 2)

Same as Phase 1: generated without network or Docker access in this
environment, so nothing here has been executed — no `pip install`
(including `langgraph`, which is new in Phase 2 and unverified),
`docker compose up`, `alembic upgrade head` against a real database, or
`pytest` run. All Python files pass a syntax check
(`python -m py_compile`) and all TS/TSX files pass a basic brace-balance
check, but that's a much lower bar than "runs correctly." Review as a
strong draft, not a verified deliverable — see the chat for the specific
list of what's confirmed vs. not.

---

## Phase 3 Verification

```bash
cd backend
alembic upgrade head
python -m scripts.seed_demo_data        # if not already run
python -m scripts.seed_observability

curl http://localhost:8000/api/analytics/dashboard -H "Authorization: Bearer <token>"
curl http://localhost:8000/api/alerts -H "Authorization: Bearer <token>"
curl http://localhost:8000/api/analytics/agents/top-performing -H "Authorization: Bearer <token>"
```

New backend tests: `test_performance_engine.py`, `test_health_score.py`,
`test_cost_engine.py`, `test_rollup_and_alerts.py`,
`test_analytics_and_alerts_api.py` — run with the existing `pytest -v`.

Frontend: `recharts` is now a dependency (added for the trend/bar/pie
charts on `/dashboard`, `/performance`, `/costs`, `/tokens`) —
`npm install` needs to succeed for the new chart components to build.

## Known Limitation of This Delivery (Phase 3)

Same as Phase 1 and 2: generated without network or Docker access, so
nothing has been executed — no `pip install`, no `npm install`
(`recharts` is new and unverified), no `docker compose up`, no
`alembic upgrade head` against a real database, no `pytest` run. All 69
backend Python files pass `python -m py_compile` and all 31 frontend
TS/TSX files pass a basic brace-balance check, but that doesn't catch
type errors, import-order issues, or runtime bugs. Review as a strong
draft — see the chat for what's confirmed vs. not.
