# Niyuvia — Non-Functional Requirements Specification

**Product:** Niyuvia  
**Product Category:** AI-Assisted Applicant Tracking System (ATS)  
**Document Type:** Non-Functional Requirements Specification  
**Version:** 1.1  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Document Owner:** Product & Engineering  
**Related Documents:** PRD.md, FUNCTIONAL_REQUIREMENTS.md, USER_WORKFLOWS.md, SECURITY_AND_PRIVACY_REQUIREMENTS.md, ARCHITECTURE.md

---

# 1. Introduction

## 1.1 Purpose

This document defines the quality attributes, measurable engineering expectations, technical constraints and operational requirements for Niyuvia.

It establishes requirements for:

- Performance and responsiveness.
- Application and infrastructure security.
- Privacy and candidate data protection.
- Reliability and data consistency.
- Scalability and tenant isolation.
- Maintainability and code quality.
- Usability and accessibility.
- Browser and API compatibility.
- Logging and observability.
- Testing and quality assurance.
- Cost efficiency.
- Deployment and operational readiness.

## 1.2 Scope

These requirements apply to:

1. The zero-budget portfolio MVP.
2. Future production development.
3. Future commercial SaaS operations.

The document distinguishes between:

- **MVP:** Requirements necessary for a safe, functional, demonstrable portfolio application.
- **Future Production:** Requirements requiring additional infrastructure, operational procedures, independent verification or financial investment.

The portfolio MVP must not be described as commercially production-ready merely because its core features work.

## 1.3 Requirement Priorities

| Priority | Meaning |
|---|---|
| P0 | Mandatory for the MVP |
| P1 | Important quality requirement; implement where practical |
| P2 | Future production enhancement |

## 1.4 Verification Principles

Every requirement must have an identifiable verification method.

Acceptable verification methods include:

- Automated unit tests.
- Integration tests.
- End-to-end tests.
- Security tests.
- Performance measurements.
- Code review.
- Manual usability testing.
- Configuration inspection.
- Documented operational checks.

A requirement must not be marked complete solely because a corresponding interface or configuration file exists.

## 1.5 Development Constraints

The initial implementation must respect these constraints:

- Development takes place on a local developer machine.
- Mandatory paid cloud infrastructure is excluded.
- Free and open-source tooling is preferred.
- Local PostgreSQL and local file storage are preferred.
- AI processing must support a mock provider.
- External AI services are optional during initial development.
- Production deployment and commercial operations are separate future activities.

---

# 2. Performance Requirements

## NFR-PERF-001 — Dashboard Responsiveness

**Priority:** P1

The dashboard should load its primary information within 3 seconds under documented MVP test conditions.

Acceptance criteria:

- Dashboard loading time can be measured.
- API requests required by the dashboard are identifiable.
- Unnecessary database queries are avoided.
- Loading and failure states are displayed.
- Performance results are recorded using a repeatable test dataset.

The 3-second target is an engineering objective, not a production SLA.

## NFR-PERF-002 — API Response Time

**Priority:** P1

Ordinary backend API operations should generally complete within 1 second under normal local MVP test conditions.

External AI processing, file extraction and other long-running operations are excluded from this target.

Acceptance criteria:

- API response times can be measured.
- Slow endpoints can be identified.
- Database query performance can be inspected.
- External provider latency is measured separately.
- Performance regressions can be detected.

## NFR-PERF-003 — Pagination

**Priority:** P0

All collection endpoints must use pagination.

Acceptance criteria:

- Collection endpoints do not return unlimited records by default.
- A default page size is configured.
- A maximum page size is enforced.
- Pagination behaviour is consistent across modules.
- Invalid pagination parameters are rejected safely.

Recommended initial defaults:

- Default page size: 20.
- Maximum page size: 100.

These values may be adjusted following implementation and testing.

## NFR-PERF-004 — Background Processing

**Priority:** P0

Long-running resume extraction and AI processing must not unnecessarily block ordinary application requests.

Acceptance criteria:

- Processing operations have identifiable statuses.
- Processing failures are recorded.
- Long-running work can be tracked independently.
- Unrelated application functionality remains usable.
- Duplicate processing is controlled.

A lightweight background-processing approach is acceptable for the MVP.

## NFR-PERF-005 — Frontend Efficiency

**Priority:** P1

The frontend should avoid unnecessary rendering, excessive client-side data loading and redundant network requests.

Acceptance criteria:

- Large lists use pagination.
- Components have clear responsibilities.
- Data fetching is controlled.
- Unnecessary repeated requests are avoided.
- Loading and error states are implemented.

## NFR-PERF-006 — Database Query Efficiency

**Priority:** P1

Database access should use appropriate indexes, constraints and query patterns.

Acceptance criteria:

- Frequently queried fields are reviewed for indexing.
- Organization-scoped queries are optimized.
- N+1 query patterns are investigated.
- Database query performance can be measured.

---

# 3. Security Requirements

Detailed security controls are also defined in `SECURITY_AND_PRIVACY_REQUIREMENTS.md`.

## NFR-SEC-001 — Secure Authentication

**Priority:** P0

The system must use a secure authentication mechanism.

Acceptance criteria:

- Authentication is enforced by the backend.
- Authentication secrets are never exposed to browser code.
- Password handling uses a trusted authentication provider or appropriate password hashing.
- Session management follows the selected authentication mechanism's security recommendations.
- Authentication failures are handled safely.

## NFR-SEC-002 — Backend Authorization

**Priority:** P0

All protected backend operations must validate the requesting user's permissions.

Acceptance criteria:

- Frontend-only access restrictions are insufficient.
- Unauthorized operations are rejected.
- Resource ownership is checked.
- Role-based permissions are enforced.
- Organization boundaries are validated.
- Authorization tests cover important protected operations.

## NFR-SEC-003 — Secret Management

**Priority:** P0

Sensitive credentials must never be committed to source control.

Examples:

- Database credentials.
- AI provider API keys.
- Authentication secrets.
- Storage credentials.
- Third-party integration credentials.

Acceptance criteria:

- Environment variables are used for secrets.
- `.env` files are excluded from Git.
- `.env.example` contains placeholders only.
- Development and production credentials are separated.
- Secrets are not included in logs or API responses.

## NFR-SEC-004 — Input Validation

**Priority:** P0

All external input must be validated before processing.

Acceptance criteria:

- Request schemas are validated.
- File uploads are validated.
- Invalid data is rejected.
- Database queries use safe parameterization or ORM mechanisms.
- Untrusted content is never executed.
- Validation errors do not expose internal implementation details.

## NFR-SEC-005 — Secure File Handling

**Priority:** P0

Resume documents must be handled securely.

Acceptance criteria:

- Supported file formats are restricted.
- File size limits are enforced.
- File content is validated where practical.
- Original filenames are not trusted as storage paths.
- Files are stored outside publicly accessible application directories.
- File access requires authorization.
- Corrupted or unsupported files fail safely.

## NFR-SEC-006 — Dependency Security

**Priority:** P1

Project dependencies should be reviewed for known vulnerabilities.

Acceptance criteria:

- Dependencies are declared through appropriate package managers.
- Lockfiles are maintained.
- Available dependency auditing tools are used.
- Critical vulnerabilities are investigated before release.
- Unnecessary dependencies are avoided.

## NFR-SEC-007 — Safe Error Responses

**Priority:** P0

Error responses must not disclose sensitive internal information.

Acceptance criteria:

- Stack traces are not exposed to ordinary users.
- Database credentials are never returned.
- Internal implementation details are not unnecessarily disclosed.
- Errors use consistent response structures.
- Internal errors are logged safely.

## NFR-SEC-008 — Rate Limiting

**Priority:** P1

Sensitive and resource-intensive endpoints should support appropriate rate limits.

Examples:

- Authentication.
- Resume upload.
- AI processing.
- Candidate evaluation.

Application-level rate limiting is acceptable for the initial MVP if its limitations are documented.

## NFR-SEC-009 — Secure Transport

**Priority:** P0 for production deployment; P2 for local-only development

Production communication must use HTTPS.

Acceptance criteria:

- Production API communication uses TLS.
- Sensitive information is not transmitted through insecure production connections.
- Secure cookie attributes are configured where cookie-based authentication is used.
- Local development exceptions are documented.

---

# 4. Privacy Requirements

## NFR-PRIV-001 — Data Minimization

**Priority:** P0

The application must collect only information necessary for its defined recruitment purposes.

Acceptance criteria:

- Optional information remains optional.
- Unnecessary sensitive information is not requested.
- Collection purposes are documented.
- Candidate information is not collected merely because it might be useful later.

## NFR-PRIV-002 — Candidate Data Protection

**Priority:** P0

Candidate information must be protected against unauthorized access.

Acceptance criteria:

- Access is permission-controlled.
- Organization boundaries are enforced.
- Resume documents remain private.
- Sensitive fields are not exposed unnecessarily.
- Access-control tests cover candidate information.

## NFR-PRIV-003 — AI Provider Data Handling

**Priority:** P0

Candidate information sent to external AI providers must follow the selected provider's data-handling terms and documented processing purposes.

Acceptance criteria:

- Only required information is transmitted.
- Provider selection is documented.
- Provider retention and training policies are reviewed.
- AI provider credentials remain server-side.
- Candidate data is not intentionally used by Niyuvia for model training without an appropriate lawful basis and explicit product decision.

## NFR-PRIV-004 — Data Retention

**Priority:** P1

The system should support documented data-retention and deletion policies.

Acceptance criteria:

- Retention expectations are documented.
- Deletion processes consider original files and derived information.
- Backup-retention limitations are documented.
- Retention behaviour can be tested.

## NFR-PRIV-005 — Data Correction and Deletion

**Priority:** P1

The product should support controlled workflows for applicable candidate-information correction and deletion requests.

Acceptance criteria:

- Requests can be recorded.
- Authorized users can review requests.
- Relevant data sources are identified.
- Request outcomes are recorded.

## NFR-PRIV-006 — Privacy Documentation

**Priority:** P1

Candidate-facing data collection workflows must provide appropriate privacy information when introduced.

Acceptance criteria:

- Data collection purposes are documented.
- Applicable privacy notices are available.
- External data processors are documented.
- Legal obligations are reviewed separately before commercial launch.

---

# 5. Reliability and Availability

## NFR-REL-001 — Graceful Failure Handling

**Priority:** P0

The system must handle failures without corrupting existing recruitment records.

Acceptance criteria:

- Failed operations return clear errors.
- Existing valid records remain intact.
- Partial operations are handled safely.
- False success responses are prevented.
- Recoverable failures are distinguishable from permanent failures.

## NFR-REL-002 — AI Provider Failure

**Priority:** P0

The application must remain usable when an external AI provider is unavailable.

Acceptance criteria:

- Provider failures are recorded.
- Existing candidate information remains accessible.
- Processing failures are communicated.
- Retry operations do not create duplicate records.
- Mock AI processing remains available for development and testing.

## NFR-REL-003 — Database Failure Handling

**Priority:** P0

Database errors must be handled safely.

Acceptance criteria:

- Failed transactions are rolled back where appropriate.
- Connection errors are recorded.
- Sensitive database information is not exposed.
- The application does not report successful persistence when a database write fails.

## NFR-REL-004 — Data Consistency

**Priority:** P0

Related recruitment records must maintain consistent relationships.

Examples:

- Candidate and resume.
- Candidate and application.
- Job and application.
- Application and interview.
- Application and stage history.

Acceptance criteria:

- Database constraints protect relationships.
- Invalid references are rejected.
- Multi-step writes use appropriate transaction boundaries.
- Deletion behaviour respects defined relationships.

## NFR-REL-005 — Idempotent Processing

**Priority:** P1

Operations that may be retried must avoid unintended duplicate effects.

Examples:

- Resume processing.
- AI evaluation.
- Application stage updates.
- Background processing jobs.

Acceptance criteria:

- Duplicate submissions are handled safely.
- Processing identifiers are maintained where appropriate.
- Retried operations do not silently create duplicate records.

## NFR-REL-006 — Backup and Recovery

**Priority:** P2

Future production deployment must provide documented backup and recovery procedures.

Acceptance criteria:

- Backup frequency is defined.
- Recovery objectives are documented.
- Restoration procedures are tested.
- Backup access is restricted.
- Backup retention is documented.

The local MVP must clearly state that a developer machine is not a production backup strategy.

---

# 6. Scalability and Architecture Requirements

## NFR-SCALE-001 — Modular Architecture

**Priority:** P0

The application must use clear module boundaries.

Initial business modules:

- Authentication.
- Organizations.
- Jobs.
- Candidates.
- Resumes.
- AI processing.
- Matching.
- Applications.
- Interviews.
- Analytics.

Acceptance criteria:

- Each module has a clear responsibility.
- Business logic is not unnecessarily duplicated.
- Module dependencies are understandable.
- Future module expansion does not require an unnecessary architectural rewrite.

## NFR-SCALE-002 — Database Scalability

**Priority:** P1

Database design should support growth without unnecessary architectural complexity.

Acceptance criteria:

- Appropriate indexes are created.
- Organization-scoped queries are considered.
- Relationships are properly constrained.
- Query performance can be evaluated.
- Database migrations are maintained.

## NFR-SCALE-003 — Background Processing

**Priority:** P1

Long-running processing should support separation from ordinary API request handling.

Acceptance criteria:

- Processing status can be persisted.
- Failures can be inspected.
- Retry behaviour is controlled.
- Processing can be separated into dedicated workers when justified.

Dedicated queues and workers may be introduced when workload requires them.

## NFR-SCALE-004 — Tenant Isolation

**Priority:** P0

The data model and authorization design must account for organization-level data isolation.

Acceptance criteria:

- Organization-owned records have identifiable ownership.
- Organization-scoped queries enforce boundaries.
- Cross-organization access is tested.
- Tenant identity is derived from trusted authentication context.
- Client-supplied organization identifiers are not trusted as authorization evidence.

Future production hardening may include database row-level security.

## NFR-SCALE-005 — Horizontal Scaling Readiness

**Priority:** P2

Future production architecture should avoid unnecessary reliance on process-local state.

Examples:

- Session storage.
- Background job state.
- File storage.
- Shared configuration.

Horizontal scaling must only be introduced when justified by actual workload.

---

# 7. Maintainability Requirements

## NFR-MAINT-001 — Clear Project Structure

**Priority:** P0

The monorepo must use a documented and consistent directory structure.

Acceptance criteria:

- Frontend and backend responsibilities are separated.
- Product and engineering documentation is organized.
- Shared configuration is managed centrally where appropriate.
- Duplicate and unnecessary files are avoided.
- Directory naming is consistent.

## NFR-MAINT-002 — Readable Code

**Priority:** P0

Source code must use consistent naming and understandable module responsibilities.

Acceptance criteria:

- Functions have focused responsibilities.
- Business logic is separated from presentation.
- API routes do not contain excessive business logic.
- Complex decisions are documented.
- Unnecessary abstraction is avoided.

## NFR-MAINT-003 — Type Safety

**Priority:** P0

TypeScript and Python type annotations must be used consistently.

Acceptance criteria:

- Frontend code uses TypeScript types.
- Backend code uses Python type hints.
- Request and response models are explicitly defined.
- Unnecessary untyped data structures are avoided.
- Type-checking tools are included in development workflows.

## NFR-MAINT-004 — Technical Documentation

**Priority:** P0

Important technical decisions and operational procedures must be documented.

Examples:

- Installation.
- Environment configuration.
- API contracts.
- Database migrations.
- AI provider configuration.
- Testing.
- Deployment.
- Architecture decisions.

Documentation must describe implemented behaviour accurately and distinguish planned functionality from existing functionality.

## NFR-MAINT-005 — Dependency Management

**Priority:** P0

Dependencies must be declared and managed through appropriate package managers.

Acceptance criteria:

- Lockfiles are maintained.
- Reproducible installation is supported.
- Unnecessary dependencies are avoided.
- Dependency updates are reviewed.

## NFR-MAINT-006 — Code Consistency

**Priority:** P1

The project should use consistent formatting, linting and naming conventions.

Acceptance criteria:

- Formatting tools are configured.
- Linting rules are documented.
- Important checks can be run through documented commands.
- Critical checks are included in CI where practical.

---

# 8. Usability Requirements

## NFR-USE-001 — Simple Navigation

**Priority:** P0

Users must be able to navigate primary recruitment workflows without unnecessary complexity.

## NFR-USE-002 — Consistent Interface

**Priority:** P1

The application should use consistent:

- Buttons.
- Forms.
- Tables.
- Dialogs.
- Navigation.
- Status indicators.
- Error messages.
- Loading indicators.

## NFR-USE-003 — Meaningful Feedback

**Priority:** P0

The interface must communicate:

- Loading.
- Success.
- Validation errors.
- Empty states.
- Processing states.
- Failure states.
- Permission errors.

## NFR-USE-004 — Responsive Layout

**Priority:** P1

The application should work on common desktop, tablet and mobile screen sizes.

Recruitment management workflows should be optimized primarily for desktop use.

## NFR-USE-005 — Explainable AI Results

**Priority:** P0

AI-assisted candidate evaluations must be understandable to authorized recruiters.

Acceptance criteria:

- Evaluation dimensions are identifiable.
- Supporting evidence is presented where available.
- Missing information is not automatically interpreted as a lack of ability.
- Evaluation limitations are communicated.
- Recruiters retain control over hiring decisions.

---

# 9. Accessibility Requirements

## NFR-ACC-001 — Keyboard Navigation

**Priority:** P1

Primary application workflows should be usable through keyboard navigation.

## NFR-ACC-002 — Form Accessibility

**Priority:** P1

Forms should provide:

- Associated labels.
- Understandable validation messages.
- Visible focus states.
- Appropriate input descriptions.
- Accessible interaction controls.

## NFR-ACC-003 — Visual Accessibility

**Priority:** P1

The interface should provide adequate contrast and must not rely exclusively on color to communicate important information.

## NFR-ACC-004 — Accessibility Target

**Priority:** P2

Future production releases should work toward WCAG 2.2 Level AA conformance, supported by appropriate testing.

---

# 10. Compatibility Requirements

## NFR-COMP-001 — Browser Support

**Priority:** P1

The frontend should support recent stable versions of major browsers.

Initial testing targets:

- Google Chrome.
- Microsoft Edge.
- Mozilla Firefox.

## NFR-COMP-002 — API Compatibility

**Priority:** P0

API changes must be managed consistently.

Acceptance criteria:

- API routes use the documented version prefix.
- Breaking changes are reviewed.
- Frontend and backend contracts remain synchronized.
- API validation behaviour is consistent.

## NFR-COMP-003 — File Compatibility

**Priority:** P0

The resume-processing module must support the documented PDF and DOCX formats.

Acceptance criteria:

- Unsupported files fail safely.
- Corrupted documents fail safely.
- Extraction failures are recorded.
- Original documents remain protected.

---

# 11. Observability and Logging

## NFR-OBS-001 — Structured Logging

**Priority:** P0

Backend logs should use a consistent structured format.

Relevant fields may include:

- Timestamp.
- Log level.
- Request ID.
- Module.
- Operation.
- Error category.

## NFR-OBS-002 — Request Identification

**Priority:** P1

Requests should have identifiable request IDs to support troubleshooting.

Acceptance criteria:

- Request IDs are generated or validated at the backend boundary.
- Related logs can be correlated.
- Request IDs do not contain sensitive personal information.

## NFR-OBS-003 — AI Processing Traceability

**Priority:** P1

AI operations should record relevant processing metadata.

Examples:

- Provider.
- Model identifier.
- Prompt version.
- Processing status.
- Duration.
- Validation result.
- Token usage where available.

Candidate resume contents and other sensitive personal information must not be written unnecessarily to application logs.

## NFR-OBS-004 — Error Monitoring

**Priority:** P1

The system should make application and processing failures discoverable during development and testing.

A paid monitoring service is not required for the zero-budget MVP.

## NFR-OBS-005 — Auditability

**Priority:** P1

Important recruitment and administrative actions should be traceable.

Examples:

- Job creation and updates.
- Candidate record changes.
- Application stage changes.
- AI evaluation generation.
- Interview feedback submission.
- Permission changes.

Audit records must not unnecessarily duplicate sensitive candidate information.

---

# 12. Testing and Quality Requirements

## NFR-TEST-001 — Unit Testing

**Priority:** P0

Important business logic must have unit tests.

Examples:

- Matching calculations.
- Input validation.
- Application state transitions.
- Permission checks.
- AI output validation.

## NFR-TEST-002 — Integration Testing

**Priority:** P0

Important backend modules must be tested together.

Examples:

- Database operations.
- Authentication and authorization.
- Resume processing.
- AI response validation.
- Application stage updates.

## NFR-TEST-003 — End-to-End Testing

**Priority:** P1

The primary recruitment journey should be verified through end-to-end testing.

## NFR-TEST-004 — Regression Testing

**Priority:** P0

Changes must not silently break previously working functionality.

Acceptance criteria:

- Existing tests are executed after relevant changes.
- Regressions are investigated.
- Critical workflows are retested.
- Failed tests are not silently ignored.

## NFR-TEST-005 — Test Data

**Priority:** P0

Development and test environments should use synthetic candidate information.

Real candidate resumes must not be used as casual test fixtures without appropriate authorization and safeguards.

## NFR-TEST-006 — AI Evaluation Testing

**Priority:** P0

AI-assisted functionality must be tested independently of the user interface.

Acceptance criteria:

- Valid structured output is accepted.
- Invalid output is rejected or handled safely.
- Missing fields are handled predictably.
- Mock provider behaviour is deterministic.
- Matching calculations are reproducible.
- Human review remains part of the hiring workflow.

## NFR-TEST-007 — Automated Quality Checks

**Priority:** P0

The repository should provide documented commands for relevant quality checks.

Examples:

- Frontend linting.
- Frontend type checking.
- Backend type checking where configured.
- Backend unit tests.
- Backend integration tests.
- Production build verification.

---

# 13. Cost Efficiency Requirements

## NFR-COST-001 — Zero-Budget Development

**Priority:** P0

The initial MVP must be developed without mandatory paid infrastructure or subscriptions.

Preferred development resources:

- Local development environment.
- Free and open-source frameworks.
- Local PostgreSQL.
- Local file storage.
- Mock AI provider.
- Automated local testing.
- Optional free-tier AI API access only when available.

The application must remain developable when no paid AI API key or cloud account is available.

## NFR-COST-002 — Optional AI Provider

**Priority:** P0

The application must support an architecture in which AI provider configuration is independent of core application functionality.

Acceptance criteria:

- Provider-specific logic is isolated.
- AI functionality can be mocked in tests.
- Missing provider credentials do not prevent unrelated development workflows.
- Provider configuration is handled through environment variables.
- AI integration failures are handled safely.

## NFR-COST-003 — Resource Awareness

**Priority:** P1

The application should avoid unnecessary API calls, excessive model usage and inefficient data processing.

Acceptance criteria:

- AI requests are made only when required.
- Repeated processing is controlled.
- Token usage is recorded where available.
- Unnecessary external service dependencies are avoided.

## NFR-COST-004 — Production Cost Visibility

**Priority:** P2

Before commercial deployment, expected operating costs should be documented.

Potential cost categories:

- Application hosting.
- Database hosting.
- File storage.
- AI provider usage.
- Email and messaging.
- Monitoring.
- Backups.
- Domain and security infrastructure.

---

# 14. Deployment and Environment Requirements

## NFR-DEP-001 — Environment Separation

**Priority:** P0

The application should distinguish development, testing and production configuration.

Acceptance criteria:

- Environment-specific configuration is documented.
- Secrets are not shared between environments.
- Production settings are not silently inherited from development.
- Local development does not require production credentials.

## NFR-DEP-002 — Reproducible Setup

**Priority:** P0

A developer should be able to install and run the application using documented instructions.

Acceptance criteria:

- Dependencies are declared.
- Lockfiles are maintained.
- Database setup is documented.
- Environment variables are documented.
- Setup instructions are tested on a clean environment where practical.

## NFR-DEP-003 — Containerization

**Priority:** P1

Docker configuration should support consistent local development.

Containerization must not become a mandatory blocker for initial development if local execution is simpler.

## NFR-DEP-004 — Production Deployment Readiness

**Priority:** P2

Before commercial deployment, the system must undergo a separate production-readiness review.

The review must cover:

- Security.
- Infrastructure.
- Backups.
- Monitoring.
- Operational procedures.
- Privacy and compliance.
- Performance.
- Recovery.
- Access management.

## NFR-DEP-005 — Database Migration Safety

**Priority:** P0

Database schema changes must be managed through versioned migrations.

Acceptance criteria:

- Migration history is maintained.
- Schema changes are reviewable.
- Destructive changes are handled carefully.
- Migration failures do not silently leave the application in an unknown state.
- Setup and migration instructions are documented.

---

# 15. Requirement Verification Matrix

| Category | Requirement IDs | Verification |
|---|---|---|
| Performance | NFR-PERF | Performance testing |
| Security | NFR-SEC | Security tests and review |
| Privacy | NFR-PRIV | Privacy and access review |
| Reliability | NFR-REL | Failure and integration tests |
| Scalability | NFR-SCALE | Architecture and query review |
| Maintainability | NFR-MAINT | Code review and linting |
| Usability | NFR-USE | Usability testing |
| Accessibility | NFR-ACC | Accessibility checks |
| Compatibility | NFR-COMP | Browser and API tests |
| Observability | NFR-OBS | Logging verification |
| Testing | NFR-TEST | Automated test reports |
| Cost | NFR-COST | Dependency and service review |
| Deployment | NFR-DEP | Setup and migration validation |

---

# 16. MVP Quality Gates

The portfolio MVP must not be considered ready for demonstration until:

1. Authentication and authorization work.
2. Organization data boundaries are tested.
3. Job and candidate workflows work.
4. Resume upload is validated and protected.
5. AI parsing handles invalid output safely.
6. Matching calculations are reproducible.
7. Human review is preserved.
8. Application stage transitions are validated.
9. Important failures are handled.
10. Core automated tests pass.
11. Environment configuration is documented.
12. No secrets are committed to the repository.
13. Database migrations can be applied successfully.
14. The application can be started using documented instructions.
15. Mock AI functionality works without paid credentials.

These gates establish portfolio MVP quality, not commercial production certification.

---

# 17. Future Production Quality Gates

Before commercial launch, additional verification is required for:

- Independent security assessment.
- Tenant isolation and authorization review.
- Backup and disaster recovery.
- Privacy and legal review.
- AI quality and bias evaluation.
- Load and performance testing.
- Monitoring and incident response.
- Retention and deletion verification.
- Infrastructure security.
- Operational support procedures.
- Production secrets management.
- Provider and subprocessors review.
- Recovery testing.
- Production deployment validation.

A production launch must not be approved solely on the basis of successful local development tests.

---

# 18. Known Constraints and Deferred Decisions

The following decisions must be revisited before production deployment:

| ID | Decision | Current Position |
|---|---|---|
| NFR-DEC-001 | Production hosting | Deferred |
| NFR-DEC-002 | Managed database | Deferred |
| NFR-DEC-003 | AI provider and commercial plan | Deferred |
| NFR-DEC-004 | Production backup strategy | Deferred |
| NFR-DEC-005 | Production monitoring provider | Deferred |
| NFR-DEC-006 | Formal load-test capacity | Deferred |
| NFR-DEC-007 | Production recovery objectives | Deferred |
| NFR-DEC-008 | Independent security assessment | Required before commercial launch |

These decisions must not be treated as implemented capabilities.

---

# 19. Document Governance

This document must remain consistent with:

- `PRD.md`
- `FUNCTIONAL_REQUIREMENTS.md`
- `MVP_SCOPE_AND_RELEASE_PLAN.md`
- `SECURITY_AND_PRIVACY_REQUIREMENTS.md`
- `BUSINESS_RULES.md`
- `PRODUCT_METRICS_AND_SUCCESS_CRITERIA.md`
- `USER_STORIES.md`
- `USER_WORKFLOWS.md`
- `ARCHITECTURE.md`

Changes to quality targets, architecture or operational expectations must be reviewed against related requirements.

Requirement changes should include:

- Requirement identifier.
- Reason for change.
- Updated priority, if applicable.
- Verification method.
- Related documents affected.

---

**Document Status:** Draft — Pending Consolidated Review  
**Version:** 1.1  
**End of Document**
