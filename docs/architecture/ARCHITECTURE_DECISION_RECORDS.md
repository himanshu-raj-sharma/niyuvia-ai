# Niyuvia — Architecture Decision Records (ADRs)

**Product:** Niyuvia  
**Document:** Architecture Decision Records  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document records important architectural and technical decisions made during the development of Niyuvia.

Each decision explains:

- The problem being addressed.
- The available options.
- The selected approach.
- The reasons behind the selection.
- The consequences and trade-offs.

The purpose is to make architectural decisions understandable, traceable and maintainable.

# 2. ADR Status Definitions

| Status | Meaning |
|---|---|
| Proposed | Decision is under consideration. |
| Accepted | Decision has been approved. |
| Superseded | A newer decision replaces it. |
| Deprecated | Decision is no longer recommended. |

# 3. ADR-001 — Monorepository Strategy

**Status:** Accepted

## Context

Niyuvia requires frontend, backend, infrastructure and documentation to evolve together.

Managing separate repositories would introduce unnecessary coordination overhead during initial development.

## Decision

Use one Git repository containing all application components.

## Selected Approach

Monorepository:

- `apps/web`
- `apps/api`
- `docs`
- `infrastructure`
- `scripts`

## Consequences

**Benefits**
- Centralized documentation.
- Easier local development.
- Coordinated changes.
- One repository to maintain.

**Trade-offs**
- Repository structure must remain organized.
- Frontend and backend dependencies must remain independently managed.

# 4. ADR-002 — Modular Monolith Backend

**Status:** Accepted

## Context

The MVP needs multiple business capabilities, but deploying each capability as an independent service would increase infrastructure and maintenance complexity.

## Decision

Use a modular monolith with clearly separated business modules.

## Selected Approach

FastAPI application containing modules such as:

- Authentication.
- Jobs.
- Candidates.
- Resumes.
- Matching.
- Recruitment pipeline.
- Interviews.
- Analytics.

## Consequences

**Benefits**
- Easier local development.
- Simpler deployment.
- Clear business boundaries.
- Lower operational overhead.

**Trade-offs**
- Module dependencies must be controlled.
- The application may eventually require separate workers or services as workload grows.

# 5. ADR-003 — Frontend Technology

**Status:** Accepted

## Context

Niyuvia requires a maintainable, responsive recruiter dashboard.

## Decision

Use Next.js with TypeScript.

## Selected Approach

- Next.js.
- React.
- TypeScript.
- Tailwind CSS.
- shadcn/ui.

## Consequences

**Benefits**
- Component-based development.
- Type safety.
- Reusable interface components.
- Strong ecosystem.

**Trade-offs**
- Developers must understand React and Next.js conventions.
- Frontend architecture must avoid unnecessary complexity.

# 6. ADR-004 — Backend Technology

**Status:** Accepted

## Context

The product requires document processing, AI integration, database operations and REST API development.

## Decision

Use Python and FastAPI.

## Selected Approach

- Python.
- FastAPI.
- Pydantic.
- SQLAlchemy.
- Alembic.

## Consequences

**Benefits**
- Python ecosystem for AI and document processing.
- Automatic OpenAPI documentation.
- Structured request validation.
- Clear service-layer organization.

**Trade-offs**
- Python dependencies require controlled version management.
- Database operations must be designed carefully.

# 7. ADR-005 — Primary Database

**Status:** Accepted

## Context

Niyuvia manages related recruitment entities, including organizations, users, jobs, candidates, applications and evaluations.

## Decision

Use PostgreSQL as the primary relational database.

## Consequences

**Benefits**
- Relational integrity.
- Transaction support.
- Mature indexing and query capabilities.
- Suitable for structured recruitment data.

**Trade-offs**
- Database migrations must be maintained.
- Relationships and indexes require deliberate design.

# 8. ADR-006 — Zero-Cost AI Development

**Status:** Accepted

## Context

Initial development must not require paid AI subscriptions or API usage.

## Decision

Use a deterministic Mock AI Provider during initial development.

## Selected Approach

The backend will define a provider-independent AI interface.

The initial implementation will use mock structured responses.

Real external AI providers may be integrated later without rewriting the entire application.

## Consequences

**Benefits**
- Zero AI API expenditure during initial development.
- Repeatable tests.
- Predictable development behavior.
- Reduced provider dependency.

**Trade-offs**
- Mock results do not demonstrate real AI accuracy.
- Real provider integration requires separate validation.

# 9. ADR-007 — Development File Storage

**Status:** Accepted

## Context

Resume processing requires file upload and retrieval functionality.

Paid object storage is not necessary for initial local development.

## Decision

Use private local filesystem storage during development.

## Future Direction

Introduce private S3-compatible object storage when public deployment and operational requirements justify it.

## Consequences

**Benefits**
- No storage subscription required.
- Easy local testing.
- Simple initial implementation.

**Trade-offs**
- Local storage is not suitable as the final production storage strategy.
- Backups and file access controls must be considered.

# 10. ADR-008 — Candidate Authentication

**Status:** Accepted

## Context

The initial MVP is an internal recruitment management application.

Candidate self-service functionality is outside the approved initial scope.

## Decision

Candidates will not have application login accounts in the initial MVP.

Recruiters will create and manage candidate records internally.

## Consequences

**Benefits**
- Smaller authentication scope.
- Simpler recruiter workflows.
- Faster initial development.

**Trade-offs**
- Candidate self-service features must be designed separately if introduced later.

# 11. ADR-009 — No Microservices in MVP

**Status:** Accepted

## Context

Microservices introduce distributed communication, deployment and observability requirements.

The initial product does not justify this complexity.

## Decision

Do not introduce microservices during MVP development.

## Future Review Conditions

Reconsider this decision only when measurable requirements justify independent deployment or scaling.

# 12. ADR-010 — Production Infrastructure Deferral

**Status:** Accepted

## Context

The developer currently has a zero-budget constraint.

Paid infrastructure must not become a dependency for completing the MVP.

## Decision

Develop and test locally using free tools and services.

Production infrastructure investments will be considered after the MVP and product validation.

## Consequences

**Benefits**
- No mandatory infrastructure expenditure.
- Full control over local development.
- Lower financial risk.

**Trade-offs**
- Local development does not demonstrate production availability, scalability or disaster recovery.

# 13. Decision Change Management

An accepted decision must not be changed silently.

When a decision needs revision:

1. Identify the original ADR.
2. Explain the reason for reconsideration.
3. Evaluate alternatives.
4. Record the new decision.
5. Mark the original ADR as superseded if appropriate.
6. Update affected architecture and engineering documentation.

# 14. Document Governance

This document must remain consistent with:

- `ARCHITECTURE.md`
- `../product/PRD.md`
- `../product/MVP_SCOPE_AND_RELEASE_PLAN.md`
- `../product/NON_FUNCTIONAL_REQUIREMENTS.md`

Significant architectural changes must be reflected in the relevant documentation.

---

**Document Status:** Draft — Pending Review

**End of Document**