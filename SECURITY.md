# Security

Implemented controls include JWT authentication, backend RBAC, agent-tool grants, deterministic governance checks, rate limiting, CORS configuration, PII masking in security/audit helpers, audit integrity chaining, suspended-agent blocking, system emergency disable, prompt-injection indicators, and structured validation.

## Final review notes
- Secrets are supplied through environment variables; `.env` is excluded from the release.
- SQLAlchemy parameterization is used for database access.
- Tool execution is registry/permission controlled rather than arbitrary shell execution.
- CORS should be restricted to the real frontend origin in production.
- Replace demo credentials and the development JWT secret before deployment.
- Rate limiting is process-local; production deployments should move this to a shared gateway/Redis strategy.
- Prompt-injection detection is heuristic and not a proof of safety.
- Demo embeddings are deterministic and not equivalent to a production-grade semantic model.
