# PERFORMANCE_SCORING.md

Documents the Phase 3 Performance Score and Health Score formulas in
full, per the instruction to keep scoring transparent and to document
the formula rather than leave it opaque.

## What these scores are — and are not

**Performance Score (0-100)** measures operational task execution
quality for one agent over a period: did its tasks succeed, how fast,
how consistently, and how well did people rate the output.

**Health Score (0-100)** measures whether an agent is operationally
sound right now: availability, failure trend, latency, and (once later
phases implement them) security events and policy violations.

**Neither score measures the underlying AI model's intelligence,
reasoning ability, or general capability.** A highly capable model
running with a bad prompt, an overloaded tool, or a flaky downstream
system will score low here — that's a statement about this deployment's
operational behavior, not about the model itself. Do not present either
score as a measure of intelligence.

## Performance Score formula

Computed daily per agent in `app/services/performance_engine.py`,
persisted to `agent_metrics.performance_score`.

```
performance_score =
    0.40 × success_rate
  + 0.20 × latency_score
  + 0.20 × reliability
  + 0.20 × evaluation_score_avg
```

- **success_rate** (0-100): `tasks_success / tasks_total × 100` for the day.
- **latency_score** (0-100): `100 - (avg_latency_ms / threshold_latency_ms_max × 100)`,
  clamped to [0, 100]. 0ms → 100; at or beyond the configured latency
  threshold → 0.
- **reliability** (0-100): the average of `success_rate` over the
  trailing 7 days (today included) — consistency over time, not just
  today's snapshot. An agent with six perfect days and one bad day
  scores lower here than a single day's success_rate would suggest.
- **evaluation_score_avg** (0-100): the average of all `agent_feedback.score`
  submitted for the agent. **If no feedback exists yet, this component
  is dropped and its 0.20 weight is redistributed proportionally across
  the other three** — the score is never assumed.

Weights are named constants in `performance_engine.py`
(`WEIGHT_SUCCESS_RATE`, etc.) — change them there if the balance needs
tuning; this doc must be kept in sync with that file.

## Health Score formula

Computed in the same daily pass, in `app/services/health_score.py`,
persisted to `agent_metrics.health_score`.

```
health_score =
    0.30 × availability
  + 0.25 × (100 - failure_rate)
  + 0.20 × latency_score
  + 0.15 × token_anomaly_component
  + 0.05 × security_events_component   (placeholder, always 100)
  + 0.05 × policy_violations_component (placeholder, always 100)
```

- **availability** (0-100): a fixed mapping from `agent.status` —
  ACTIVE/IDLE/BUSY → 100, TRAINING/WAITING → 60, SUSPENDED/OFFLINE → 30,
  FAILED/RETIRED → 0. See `_AVAILABILITY_BY_STATUS` in `health_score.py`.
- **failure_rate**: same definition as in the Performance Score.
- **latency_score**: same function as in the Performance Score.
- **token_anomaly_component**: 100 normally, 40 if the agent currently
  has an unresolved `TOKEN_BUDGET_EXCEEDED` or `TOKEN_SPIKE` alert.
- **security_events_component / policy_violations_component**: fixed
  at 100 — **explicit placeholders**. There is no `security_events`
  table or governance/policy module yet (both are future-phase work per
  ARCHITECTURE.md). These components exist in the formula now so the
  weight allocation doesn't need to change later, but they currently
  contribute a constant, never a penalty. This is a placeholder, not a
  claim that an agent has no security events or policy violations.

## Where these get computed

`app/services/rollup.py::recompute_daily_rollups()` aggregates a day's
`task_runs` into `agent_metrics` / `token_usage` / `costs`, then
`app/services/alert_engine.py::process_agent_day()` runs both the
rollup and the alert threshold checks together — this is the one
function everything (task-run recording, the Phase 2 `/execute`
endpoint, and the seed scripts) calls to keep numbers consistent.

## Cost calculation

Cost is never hard-coded per provider. `app/services/cost_engine.py`
reads `$/1k input tokens` and `$/1k output tokens` from the
`model_pricing` table (configurable via `POST /api/model-pricing`,
admin-only). If a model has no pricing row and isn't one of the three
bootstrap defaults seeded for demo purposes, cost is reported as `0.0`
rather than guessed.

## Alert thresholds

All thresholds (`performance_score_min`, `failure_rate_max`,
`latency_ms_max`, `daily_token_budget`, `daily_cost_budget`,
`monthly_cost_budget`, `token_spike_ratio`, `tokens_per_task_max`) are
environment-configurable in `app/config.py` / `.env` — not hard-coded
inside `alert_engine.py`. Current values are readable (not writable
via API in Phase 3) at `GET /api/alerts/thresholds`.
