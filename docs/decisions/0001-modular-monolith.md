# ADR 0001: Use a modular monolith

- Status: accepted
- Date: 2026-07-15

## Context

The project needs API, data ingestion, AI enrichment, recommendation, and application-tracking capabilities. It is initially built and operated by one developer and has no demonstrated scaling bottleneck.

## Decision

Use one Python backend repository and deployment unit, separated internally by domain and adapter boundaries. Keep the web client as a distinct application because it has a different language, build chain, and runtime.

## Consequences

- Local development, testing, refactoring, and deployment remain simple.
- Database transactions across domains are straightforward.
- Clear module boundaries preserve the option to extract a worker or service later.
- Architectural discipline must be enforced in code review and tests because process boundaries do not enforce it automatically.
