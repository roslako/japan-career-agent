# Japan Job Tracker

Project for discovering, organizing, and evaluating graduate and entry-level software roles in Japan.

## Current phase

This repository contains architecture and project scaffolding only. There is no business logic yet.

## Product direction

The tracker is intended to eventually:

- collect roles from approved job sources;
- normalize Japanese and English job information;
- track companies, applications, and application status;
- match roles to a candidate profile using explainable AI assistance; and
- surface useful insights without making application decisions for the user.

## Technology choices

| Area | Choice | Reason |
| --- | --- | --- |
| Backend | Python 3.12 + FastAPI | Builds on existing Python knowledge; typed, async-ready, and well suited to APIs and AI/data work. |
| Data access | SQLAlchemy 2 + Alembic | Explicit database models and production-grade schema migrations. |
| Database | PostgreSQL | Job, company, and application data is relational; PostgreSQL also supports full-text search and optional `pgvector` later. |
| Background work | Task abstraction, with Redis + Celery deferred | Collection and AI enrichment will become background jobs, but infrastructure is postponed until a real asynchronous workflow exists. |
| Frontend | Next.js + TypeScript | Strong React ecosystem, type safety, routing, and a polished portfolio-friendly UI path. |
| AI | Provider adapter, model undecided | Prevents core product code from depending directly on one model vendor; a model should be selected when the first AI use case is defined. |
| Data validation | Pydantic | Shared typed boundaries for API input, configuration, and normalized job data. |
| Python tooling | `uv`, Ruff, mypy, pytest | Fast dependency management plus formatting, linting, type checking, and testing. |
| Web tooling | pnpm, ESLint, Vitest, Playwright | Fast reproducible installs and layered frontend testing. |
| Local environment | Docker Compose | Reproducible PostgreSQL locally without forcing the apps themselves into containers during early development. |
| CI | GitHub Actions | Visible, standard quality checks for a portfolio repository. |

## Repository map

```text
.
├── apps/
│   ├── api/                 # Python API and future background tasks
│   │   ├── src/job_tracker/
│   │   │   ├── api/         # HTTP-only concerns
│   │   │   ├── core/        # Configuration and cross-cutting policies
│   │   │   ├── db/          # Persistence setup and migrations boundary
│   │   │   ├── domains/     # Business modules, kept independent
│   │   │   ├── integrations/# External job sources and AI providers
│   │   │   └── workers/     # Background-task entry points
│   │   └── tests/
│   └── web/                 # Next.js user interface
├── docs/
│   ├── architecture/        # System and domain design
│   └── decisions/           # Architecture decision records (ADRs)
├── infra/docker/            # Local/deployment container assets
├── scripts/                 # Repeatable developer commands
└── .github/workflows/       # Continuous integration definitions
```

## Architectural rules

1. Start as a modular monolith. Domain modules may not import HTTP or vendor-specific integration code.
2. External job boards and AI services sit behind adapters in `integrations/`.
3. Store original source data alongside normalized fields so transformations remain auditable.
4. AI output is advisory and explainable. Deterministic filtering and database queries remain ordinary code.
5. Add infrastructure only when a demonstrated requirement needs it.

## Suggested next milestone

Define the first vertical slice: manually create, list, and inspect a job. Before coding, write its user story, data model, API contract, and acceptance criteria.

See [the architecture overview](docs/architecture/overview.md) and [ADR 0001](docs/decisions/0001-modular-monolith.md).
