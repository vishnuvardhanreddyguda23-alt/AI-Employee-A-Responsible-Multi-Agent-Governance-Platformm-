# Database

PostgreSQL 16 is the default database. The Compose image includes pgvector. Alembic migrations are the source of truth and run automatically on container startup.

The Phase 5 knowledge schema stores document chunks with deterministic demo embeddings and includes a pgvector-ready column/extension for production embedding migration.

Fresh database:
```bash
docker compose down -v
docker compose up --build
```

Do not use `Base.metadata.create_all()` as the production migration mechanism.
