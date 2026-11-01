# Niyuvia — Security and Privacy Requirements

**Product:** Niyuvia  
**Document:** Security and Privacy Requirements  
**Version:** 1.1  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Document Owner:** Product & Engineering  
**Related Documents:**
- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md

---

# 1. Purpose

This document defines the security and privacy requirements for Niyuvia.

It establishes requirements for protecting:

- User accounts and authentication credentials.
- Organization information and recruitment records.
- Candidate personal information.
- Uploaded resumes and extracted information.
- AI processing inputs, outputs and metadata.
- Organization-level data boundaries.
- Application infrastructure and configuration.
- Security logs and audit records.

These requirements apply to the zero-budget portfolio MVP and establish a foundation for future commercial production.

The document distinguishes between controls required during local MVP development and additional controls required before commercial production.

The existence of this document does not establish legal compliance, security certification or production readiness.

# 2. Scope

## 2.1 In Scope

The security requirements cover:

1. Authentication and session management.
2. Authorization and role-based access control.
3. Organization and tenant isolation.
4. API security.
5. Resume and document security.
6. AI processing security.
7. Candidate privacy.
8. Database security.
9. Logging and auditing.
10. Application and infrastructure security.
11. Security testing.
12. Incident response.
13. Production security readiness.

## 2.2 MVP Scope

The initial MVP is a recruiter-facing recruitment management application.

The MVP does not provide:

- Candidate login accounts.
- Candidate self-service portals.
- Public candidate registration.
- Public job application submission.
- Autonomous candidate rejection.
- Autonomous employment decisions.

Candidate information is initially managed by authorized internal recruiters.

Candidate access, correction and deletion requests must be handled through controlled internal processes.

## 2.3 Future Production Scope

Commercial production must introduce additional controls for:

- Multiple independent customer organizations.
- Production authentication and identity management.
- Secure cloud infrastructure.
- Private object storage.
- Production backup and recovery.
- Operational security monitoring.
- Formal incident response.
- Applicable privacy and data-protection obligations.

# 3. Security Objectives

Niyuvia must protect:

1. Confidentiality of candidate and organization information.
2. Integrity of recruitment records.
3. Availability of essential recruitment workflows.
4. Authentication credentials and application secrets.
5. Uploaded documents and extracted information.
6. AI processing inputs and outputs.
7. Organization-level data boundaries.
8. Privacy rights and candidate information.
9. Traceability of sensitive operations.
10. Protection against unauthorized access and manipulation.

# 4. Security Principles

The application must follow these principles:

- Secure by default.
- Least-privilege access.
- Backend-enforced authorization.
- Defense in depth.
- Data minimization.
- Explicit trust boundaries.
- Secure handling of untrusted input.
- Privacy-aware system design.
- Traceable sensitive operations.
- No unnecessary exposure of personal information.
- Human oversight of AI-assisted hiring decisions.
- Separation of development and production environments.
- Fail securely when authorization or validation fails.

# 5. Authentication Security

## SEC-AUTH-001 — Secure Authentication

Authentication must use a trusted authentication mechanism.

The implementation must use a maintained authentication library or trusted identity provider.

**Acceptance criteria:**
- Protected endpoints reject unauthenticated requests.
- Authentication identity is verified by the backend.
- Authentication failures do not grant partial access.

## SEC-AUTH-002 — Password Protection

If password authentication is implemented directly, passwords must be stored using an appropriate modern password-hashing algorithm.

Plaintext passwords and reversible password storage are prohibited.

**Acceptance criteria:**
- Passwords are never stored in plaintext.
- Password hashes are generated using an approved password-hashing mechanism.
- Password values are excluded from application logs.

## SEC-AUTH-003 — Session Protection

Sessions must use secure handling appropriate to the selected authentication architecture.

For cookie-based sessions, production configuration must consider:

- Secure.
- HttpOnly.
- Appropriate SameSite attributes.
- Session expiration.
- Session invalidation.

## SEC-AUTH-004 — Session Expiration

Expired and revoked sessions must not authorize protected requests.

## SEC-AUTH-005 — Authentication Errors

Authentication errors must not unnecessarily reveal whether sensitive account information exists.

## SEC-AUTH-006 — Secret Protection

Authentication secrets, signing keys and tokens must never be committed to source control or exposed through frontend bundles.

## SEC-AUTH-007 — Account Recovery

If password-based authentication is implemented, password recovery must use a secure verification process.

**Acceptance criteria:**
- Password-reset tokens must be unpredictable.
- Reset tokens must expire.
- Reset tokens must be single-use.
- Password-reset responses must not unnecessarily reveal account existence.
- Reset credentials must never appear in application logs.

## SEC-AUTH-008 — Brute-Force Protection

Authentication endpoints must implement appropriate protection against repeated failed login attempts.

Production implementations must use rate limiting and suitable abuse-detection controls.

## SEC-AUTH-009 — Session Revocation

Users must be able to have active sessions invalidated after account compromise, membership revocation or administrative security action.

## SEC-AUTH-010 — Multi-Factor Authentication Readiness

Production authentication architecture should support multi-factor authentication.

MFA is a production hardening requirement and is not mandatory for the initial local-only portfolio MVP.

# 6. Authorization and Organization Isolation

## SEC-AUTHZ-001 — Backend Authorization

Every protected backend operation must independently verify authorization.

Frontend-only access restrictions are insufficient.

## SEC-AUTHZ-002 — Role-Based Access Control

Access must be determined by assigned roles and permissions.

Initial roles:

- Organization Administrator.
- Recruiter.
- Hiring Manager.

Permissions must be explicitly defined rather than inferred from frontend visibility.

## SEC-AUTHZ-003 — Organization Isolation

Users must access only records belonging to organizations for which they have appropriate authorization.

## SEC-AUTHZ-004 — Trusted Tenant Context

The backend must derive organization context from verified authentication and membership information.

Client-supplied organization identifiers must not independently establish access rights.

## SEC-AUTHZ-005 — Resource Ownership

Authorization must be checked for individual resources, not only API routes.

This includes:

- Jobs.
- Candidates.
- Resumes.
- Applications.
- Interviews.
- Interview feedback.
- AI evaluations.

## SEC-AUTHZ-006 — Cross-Tenant Testing

Automated tests must verify that users cannot access another organization's records.

## SEC-AUTHZ-007 — Membership Revocation

Revoked organization membership must prevent subsequent unauthorized access.

## SEC-AUTHZ-008 — Privilege Separation

Administrative privileges must not automatically be granted to ordinary recruiters or hiring managers.

## SEC-AUTHZ-009 — Insecure Direct Object Reference Protection

Changing an identifier in an API request must not allow access to another user's or organization's resource.

## SEC-AUTHZ-010 — Authorization Denial

Unauthorized operations must fail without revealing protected resource contents.

# 7. API Security

## SEC-API-001 — Input Validation

All incoming API data must be validated using explicit request schemas.

## SEC-API-002 — Output Filtering

API responses must contain only information necessary for the requested operation and authorized user.

## SEC-API-003 — SQL Injection Protection

Database queries must use safe parameterized query mechanisms or ORM-supported query construction.

Unsafe dynamic SQL construction is prohibited.

## SEC-API-004 — Rate Limiting

Sensitive and expensive endpoints must have appropriate rate limits.

Priority endpoints include:

- Authentication.
- Resume uploads.
- AI parsing.
- Candidate evaluation.
- Password recovery.

## SEC-API-005 — CORS Configuration

Cross-origin access must use an explicit allowlist appropriate to each environment.

Unrestricted origins must not be used in production.

## SEC-API-006 — HTTP Security

Production deployment must use HTTPS and appropriate security headers.

## SEC-API-007 — Error Handling

API errors must not disclose credentials, internal stack traces, database details or sensitive personal information.

## SEC-API-008 — Request Size Limits

Request bodies and uploaded files must have configured size limits.

## SEC-API-009 — CSRF Protection

If cookie-based authentication is used, state-changing requests must receive appropriate Cross-Site Request Forgery protection.

The implementation must use an appropriate CSRF token strategy and cookie configuration.

## SEC-API-010 — SSRF Protection

Any future feature that retrieves external URLs must validate destinations and restrict access to private, loopback, link-local and internal infrastructure addresses.

The system must not blindly fetch URLs supplied by users or AI-generated content.

## SEC-API-011 — HTTP Method Restrictions

Endpoints must accept only the HTTP methods required by their intended operations.

Unsupported methods must be rejected.

## SEC-API-012 — API Version Security

API versioning must not bypass authentication, authorization or validation requirements.

## SEC-API-013 — Sensitive Response Protection

Sensitive responses must use appropriate cache-control policies.

Candidate personal information and resume contents must not be stored in public browser or intermediary caches.

# 8. Resume and File Security

## SEC-FILE-001 — Allowed File Types

The MVP must accept only explicitly supported resume formats:

- PDF.
- DOCX.

## SEC-FILE-002 — File Content Validation

File validation must not rely exclusively on filename extensions or browser-provided MIME types.

The backend must inspect file content using suitable validation mechanisms.

## SEC-FILE-003 — File Size Restrictions

Maximum upload size must be configured and enforced by the backend.

The application must reject files exceeding the configured limit.

## SEC-FILE-004 — Safe Storage Names

Uploaded filenames must not be used directly as trusted filesystem paths.

The application must generate safe internal storage identifiers.

## SEC-FILE-005 — Private Storage

Candidate resumes must be stored outside publicly accessible application directories.

## SEC-FILE-006 — Authorized Downloads

Resume access must require authorization and resource ownership validation.

## SEC-FILE-007 — Malicious Document Handling

Document processing must account for malformed files and potentially malicious content.

Production infrastructure should support malware scanning before documents are made available to downstream processing.

## SEC-FILE-008 — Original File Integrity

The original uploaded document must remain unchanged during extraction and parsing.

## SEC-FILE-009 — Temporary Files

Temporary processing files must be cleaned up according to a documented policy.

## SEC-FILE-010 — File Quarantine

Newly uploaded documents should remain in a restricted processing state until validation succeeds.

Production deployment must introduce an appropriate quarantine and malware-scanning workflow.

## SEC-FILE-011 — Document Processing Limits

PDF and DOCX processing must have resource limits.

These limits should include:

- Maximum file size.
- Maximum processing duration.
- Maximum extracted text size.
- Maximum document page count where applicable.
- Memory and CPU limits for processing workers.

Malformed or excessively expensive documents must fail safely.

## SEC-FILE-012 — Archive and Parser Safety

Document extraction libraries must be maintained and configured to avoid unsafe archive extraction, path traversal and resource-exhaustion vulnerabilities.

## SEC-FILE-013 — Download Filename Safety

Downloaded files must use safe response headers and must not allow user-controlled filenames to manipulate HTTP headers.

# 9. AI Security

## SEC-AI-001 — Untrusted AI Inputs

Resume contents and job descriptions must be treated as untrusted data.

## SEC-AI-002 — Prompt Injection Protection

Instructions embedded inside candidate documents must not override system instructions or authorize tools and operations.

## SEC-AI-003 — Structured Output Validation

AI responses must pass schema validation before entering trusted application workflows.

Invalid output must be rejected or safely handled.

## SEC-AI-004 — Human Review

AI-generated candidate information must remain subject to recruiter review and correction.

## SEC-AI-005 — No Autonomous Rejection

AI-generated scores and assessments must not independently reject candidates or make final employment decisions.

## SEC-AI-006 — Provider Key Protection

AI provider credentials must remain server-side.

They must never be exposed through browser code, frontend environment variables or public API responses.

## SEC-AI-007 — Provider Failure Isolation

AI provider failures must not expose sensitive information or corrupt existing recruitment records.

## SEC-AI-008 — Processing Traceability

Relevant AI metadata must be recorded, including:

- Provider.
- Model identifier.
- Prompt version.
- Processing status.
- Duration.
- Validation result.
- Token usage where available.
- Relevant evaluation configuration version.

## SEC-AI-009 — Data Minimization

Only the information required for an approved AI operation should be sent to an external provider.

## SEC-AI-010 — Provider Data Policies

Before enabling an external provider for real candidate information, its data retention, processing, security and model-training terms must be reviewed.

## SEC-AI-011 — Bias and Discrimination Safeguards

Candidate evaluation must not use protected characteristics or inappropriate proxies for protected characteristics.

Examples include:

- Religion.
- Caste.
- Race.
- Ethnicity.
- Gender.
- Disability.
- Marital status.
- Other legally protected characteristics.

Candidate evaluation must focus on job-related qualifications and documented evidence.

## SEC-AI-012 — Evidence-Based Evaluation

AI-generated candidate assessments must identify the supporting resume or application evidence wherever possible.

The system must distinguish between:

- Evidence found.
- Evidence not found.
- Information not provided.
- Information requiring human verification.

Missing information must not automatically be interpreted as proof that a candidate lacks a skill.

## SEC-AI-013 — AI Output Provenance

AI-generated records must retain sufficient metadata to identify their processing origin.

Where applicable, this includes:

- Source document identifier.
- Model and provider.
- Prompt version.
- Processing timestamp.
- Schema version.
- Evaluation configuration version.

## SEC-AI-014 — AI Output Isolation

AI-generated content must not directly execute database operations, change application permissions or perform sensitive recruitment actions.

Sensitive actions must pass through authorized application services.

## SEC-AI-015 — AI Data Retention

AI prompts, responses and intermediate processing data must follow documented retention and deletion rules.

Full resume contents must not be copied unnecessarily into logs or diagnostic records.

## SEC-AI-016 — AI Evaluation Transparency

The application must communicate that AI-generated matching scores are decision-support information, not a probability of hiring success or an objective measure of candidate worth.

# 10. Candidate Privacy Requirements

## SEC-PRIV-001 — Purpose Limitation

Candidate information must be collected and processed for documented recruitment purposes.

## SEC-PRIV-002 — Data Minimization

The application must avoid collecting unnecessary personal and sensitive information.

## SEC-PRIV-003 — Privacy Information

Appropriate privacy information must be provided at relevant candidate data collection points.

For the initial internal candidate-management MVP, the organization must have an appropriate process for communicating applicable privacy information to candidates.

## SEC-PRIV-004 — Access and Correction

Applicable candidate access and correction requests must be supported through controlled processes.

## SEC-PRIV-005 — Deletion Requests

Applicable deletion requests must be processed after checking authorization, legal retention requirements and relevant exceptions.

## SEC-PRIV-006 — Retention Policy

The system must support documented retention rules for:

- Candidate profiles.
- Resumes.
- Applications.
- Interview feedback.
- AI-generated information.
- Relevant audit records.

## SEC-PRIV-007 — Data Export

Future production functionality should support authorized candidate data export where required.

## SEC-PRIV-008 — External Processing

External service providers must be reviewed before receiving candidate personal information.

## SEC-PRIV-009 — No Unapproved Training

Candidate resumes must not be used as training data by Niyuvia without a separately reviewed and authorized purpose.

## SEC-PRIV-010 — Privacy by Design

New features must be reviewed for their impact on candidate privacy before implementation.

## SEC-PRIV-011 — Retention Lifecycle

The application must define a retention lifecycle for candidate information.

The lifecycle must cover:

1. Collection.
2. Active recruitment use.
3. Inactive records.
4. Retention review.
5. Deletion or anonymization.
6. Backup expiration where applicable.

Exact retention periods must be configurable or documented by the responsible organization.

## SEC-PRIV-012 — Deletion Propagation

When an authorized deletion request is approved, the application must identify relevant candidate information stored across:

- Candidate profiles.
- Resumes.
- Applications.
- AI extraction results.
- AI evaluation records.
- Relevant derived data.

Deletion must follow applicable legal retention obligations and documented exceptions.

## SEC-PRIV-013 — External AI Provider Data Processing

Before sending real candidate data to an external AI provider, the organization must review:

- Data processing terms.
- Data retention.
- Provider access controls.
- Model training policies.
- Processing locations where relevant.
- Subprocessors.
- Available deletion mechanisms.
- Security incident procedures.

## SEC-PRIV-014 — Candidate Data Access Restrictions

Candidate personal information must only be accessible to authorized users who require it for their recruitment responsibilities.

## SEC-PRIV-015 — Privacy Request Tracking

Requests involving candidate information must have a traceable status and record of handling.

## SEC-PRIV-016 — Candidate Data Isolation

Candidate data belonging to one organization must not be exposed to another organization.

# 11. Database Security

## SEC-DB-001 — Credential Protection

Database credentials must be managed through environment configuration or an appropriate secrets-management system.

## SEC-DB-002 — Least Privilege

Database accounts must receive only the permissions necessary for their intended responsibilities.

## SEC-DB-003 — Referential Integrity

Database constraints must protect critical relationships.

## SEC-DB-004 — Tenant-Aware Queries

Organization-owned records must be queried using validated organization context.

## SEC-DB-005 — Sensitive Data Handling

Sensitive personal information must not be unnecessarily duplicated across database records.

## SEC-DB-006 — Database Backups

Future production databases must have documented backup, restoration and access-control procedures.

## SEC-DB-007 — Encryption

Production database communication must use encrypted connections.

Production storage must use appropriate encryption-at-rest controls.

## SEC-DB-008 — Backup Restoration Testing

Production backup procedures must include restoration testing.

A backup must not be considered reliable merely because its creation operation succeeded.

## SEC-DB-009 — Database Migration Safety

Database migrations must be reviewed and tested before being applied to production data.

Destructive migrations must have an approved recovery or migration strategy.

## SEC-DB-010 — Tenant Data Constraints

Where tenant-owned entities are related, database relationships should enforce tenant consistency wherever practical.

Application-level authorization must remain mandatory even when database constraints exist.

# 12. Logging and Audit Security

## SEC-LOG-001 — Structured Logging

Security-relevant events should be logged using structured logging.

## SEC-LOG-002 — Sensitive Data Exclusion

Logs must not contain:

- Passwords.
- Authentication tokens.
- API secrets.
- Full resume contents.
- Unnecessary candidate personal information.
- Private document contents.

## SEC-LOG-003 — Security Events

Relevant events should include:

- Authentication failures.
- Authorization failures.
- Permission changes.
- Sensitive data access where appropriate.
- Resume upload and download activity.
- Candidate information changes.
- Application stage changes.
- Deletion requests.
- AI processing failures.
- Administrative security actions.

## SEC-LOG-004 — Audit Integrity

Audit records must be protected against unauthorized modification and deletion.

## SEC-LOG-005 — Request Correlation

Security and operational events should include request identifiers where practical.

## SEC-LOG-006 — Audit Retention

Audit retention periods must be documented and aligned with operational and applicable legal requirements.

## SEC-LOG-007 — Audit Access Control

Only authorized administrative roles and approved operational processes may access sensitive audit records.

## SEC-LOG-008 — Log Injection Protection

Untrusted user input must be safely encoded or sanitized before being included in logs.

# 13. Application and Infrastructure Security

## SEC-INFRA-001 — Environment Separation

Development, testing and production environments must use separate configuration and credentials.

## SEC-INFRA-002 — Git Protection

Environment files containing secrets, private keys and sensitive test data must be excluded from Git.

## SEC-INFRA-003 — Dependency Review

Dependencies must be periodically checked for known vulnerabilities.

## SEC-INFRA-004 — Secure Defaults

Debug mode, permissive CORS and development credentials must not be enabled in production.

## SEC-INFRA-005 — TLS

External production communication must use HTTPS.

## SEC-INFRA-006 — Backup Protection

Production backups must receive appropriate access controls and protection.

## SEC-INFRA-007 — Secret Rotation

Production secrets and credentials must support controlled rotation.

Compromised credentials must be revocable without requiring a complete application redesign.

## SEC-INFRA-008 — Dependency Security Checks

The development workflow should include automated dependency vulnerability checks.

Critical vulnerabilities affecting deployed dependencies must be reviewed before release.

## SEC-INFRA-009 — Secure Configuration

Environment-specific settings must be validated during application startup.

The application must fail safely when mandatory production configuration is missing or invalid.

## SEC-INFRA-010 — Infrastructure Access Control

Production infrastructure access must be restricted to authorized personnel and protected administrative accounts.

## SEC-INFRA-011 — Webhook Verification

If external webhooks are introduced, incoming webhook requests must be authenticated using the provider's supported signature-verification mechanism.

## SEC-INFRA-012 — Webhook Replay Protection

If external webhooks are introduced, the application must prevent duplicate or replayed events from causing unintended repeated operations.

# 14. Security Testing Requirements

The following tests are mandatory before considering the relevant MVP functionality verified.

| Test ID | Test Scenario | Expected Result |
|---|---|---|
| SEC-TC-001 | Unauthenticated access | Request rejected |
| SEC-TC-002 | Unauthorized role access | Request rejected |
| SEC-TC-003 | Cross-organization record access | Access denied |
| SEC-TC-004 | Invalid session | Access denied |
| SEC-TC-005 | Invalid request payload | Validation error |
| SEC-TC-006 | Unsupported resume file | Upload rejected |
| SEC-TC-007 | Oversized resume | Upload rejected |
| SEC-TC-008 | Unauthorized resume download | Access denied |
| SEC-TC-009 | Invalid AI response | Response rejected |
| SEC-TC-010 | AI provider unavailable | Failure handled safely |
| SEC-TC-011 | Secret exposed in frontend | Security test fails |
| SEC-TC-012 | Sensitive information in logs | Security test fails |
| SEC-TC-013 | Invalid application stage change | Transition rejected |
| SEC-TC-014 | Unauthorized candidate update | Update rejected |
| SEC-TC-015 | Unauthorized deletion request | Operation rejected |
| SEC-TC-016 | Expired password-reset token | Reset rejected |
| SEC-TC-017 | Repeated failed authentication | Rate limiting applied |
| SEC-TC-018 | CSRF attack against cookie-authenticated endpoint | Request rejected |
| SEC-TC-019 | Unsafe external URL | Request rejected |
| SEC-TC-020 | Malformed document | Processing fails safely |
| SEC-TC-021 | Excessive document-processing workload | Resource limits enforced |
| SEC-TC-022 | Prompt injection inside resume | Embedded instructions treated as untrusted |
| SEC-TC-023 | Unsupported candidate evaluation criteria | Evaluation rejected |
| SEC-TC-024 | AI output without evidence | Output marked appropriately |
| SEC-TC-025 | Unauthorized audit-log access | Access denied |
| SEC-TC-026 | Database migration failure | No silent data corruption |
| SEC-TC-027 | Secret rotation | Old credential rejected after rotation |
| SEC-TC-028 | Duplicate webhook event | Duplicate operation prevented when webhooks are enabled |
| SEC-TC-029 | Candidate deletion propagation | Applicable records handled according to policy |
| SEC-TC-030 | Cross-tenant resume access | Access denied |

# 15. Incident Response Requirements

Future production operations must define procedures for:

1. Identifying a suspected security incident.
2. Limiting further unauthorized access.
3. Investigating the affected systems and information.
4. Preserving relevant evidence.
5. Assessing impact.
6. Taking corrective action.
7. Communicating with affected parties where required.
8. Reviewing the incident and preventing recurrence.

Applicable legal notification requirements must be reviewed before commercial launch.

Incident response procedures must identify responsible personnel and escalation channels.

# 16. Zero-Budget MVP Security Strategy

The initial development must not require paid security infrastructure.

The following free or local approaches should be used where practical:

- Local PostgreSQL.
- Environment-based secret configuration.
- Backend authorization checks.
- Pydantic request validation.
- Automated security tests.
- Dependency vulnerability scanning.
- Private local resume storage.
- Synthetic candidate data.
- Mock AI provider for tests.
- Git-based code review.
- Local application logging.
- Version-controlled database migrations.
- Automated test execution.

## 16.1 Development Data Restrictions

During zero-budget development:

- Use synthetic candidate profiles.
- Use synthetic resumes.
- Avoid uploading real candidate personal information.
- Do not commit local database files containing personal information.
- Do not commit `.env` files.
- Do not expose AI provider credentials.
- Do not treat local storage as production-grade private storage.

## 16.2 Mock AI Provider

The initial development environment should use a mock AI provider wherever practical.

The mock provider must return deterministic structured responses so that parsing, validation and matching workflows can be tested without paid AI API usage.

## 16.3 Cost Restrictions

The initial MVP must avoid mandatory paid dependencies.

Paid services may be evaluated later when the project moves toward commercial production.

A local development environment must never be treated as equivalent to a secured commercial production environment.

# 17. Production Security Readiness

Before commercial launch, Niyuvia requires a separate security and privacy readiness review covering:

- Authentication architecture.
- Account recovery.
- MFA readiness.
- Role and permission model.
- Tenant isolation.
- Database access.
- File storage security.
- Malware scanning.
- AI provider agreements and configuration.
- Data retention and deletion.
- Encryption and key management.
- Backup and recovery.
- Audit-log protection.
- Vulnerability assessment.
- Incident response.
- Monitoring and alerting.
- Secret rotation.
- External service providers.
- Applicable privacy and legal obligations.

Production launch must not proceed merely because the application builds successfully or its automated tests pass.

The production readiness review must verify the actual deployed configuration.

# 18. Security Acceptance Criteria

The security requirements must be considered implemented only when the relevant controls have been verified.

The following conditions apply:

1. Authentication must be tested through protected backend endpoints.
2. Authorization must be enforced on the backend.
3. Cross-organization access must be covered by automated tests.
4. Resume upload and download must enforce access restrictions.
5. AI outputs must pass schema validation.
6. AI matching must not independently reject candidates.
7. Candidate information must not appear unnecessarily in logs.
8. Secrets must not be committed to source control.
9. Environment configuration must be separated.
10. Relevant security tests must pass.
11. Documented limitations must remain visible.
12. Production-only requirements must not be marked complete during local development.

Each requirement must have an implementation status:

- NOT_STARTED
- IN_PROGRESS
- IMPLEMENTED
- VERIFIED
- DEFERRED

IMPLEMENTED and VERIFIED are different states.

A requirement must not be marked VERIFIED without appropriate evidence, such as an automated test, manual test record or configuration review.

# 19. Security Risk Management

Security risks must be documented and reviewed during development.

Initial risk categories include:

| Risk ID | Risk | Mitigation |
|---|---|---|
| RISK-SEC-001 | Unauthorized candidate data access | Backend authorization and tenant isolation |
| RISK-SEC-002 | Resume upload abuse | File validation and processing limits |
| RISK-SEC-003 | AI prompt injection | Untrusted input handling and output validation |
| RISK-SEC-004 | AI provider data exposure | Data minimization and provider review |
| RISK-SEC-005 | Credential leakage | Environment configuration and Git protection |
| RISK-SEC-006 | Cross-tenant data leakage | Tenant-aware queries and automated tests |
| RISK-SEC-007 | Inappropriate candidate evaluation | Job-related criteria and human review |
| RISK-SEC-008 | Accidental data deletion | Controlled deletion and recovery procedures |
| RISK-SEC-009 | Sensitive information in logs | Structured logging and redaction |
| RISK-SEC-010 | Dependency vulnerabilities | Dependency scanning and maintenance |

# 20. Document Governance

This document must remain consistent with:

- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md

Changes affecting authentication, authorization, candidate data, AI processing, file handling or infrastructure security must trigger a security review.

Changes to security requirements must be reflected in the requirements traceability matrix.

Requirement identifiers must remain stable unless a documented change requires otherwise.

---

**Document Status:** Draft — Pending Review

**Version:** 1.1

**End of Document**