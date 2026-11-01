# Niyuvia — Secure Coding Standards

**Product:** Niyuvia  
**Document:** Secure Coding Standards  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Owner:** Engineering and Security

---

# 1. Purpose

This document defines the secure software development practices required throughout Niyuvia.

Its objectives are to:

- Protect candidate and organizational information.
- Prevent common application vulnerabilities.
- Establish consistent security practices.
- Protect authentication credentials and application secrets.
- Enforce authorization at every relevant system boundary.
- Reduce security risks in AI-powered workflows.
- Establish a foundation for future production deployment.

Security must be considered throughout development rather than added after implementation.

# 2. Security Principles

All development must follow these principles:

1. Secure by default.
2. Least-privilege access.
3. Defense in depth.
4. Explicit authorization.
5. Input validation at system boundaries.
6. Safe handling of sensitive information.
7. Fail securely.
8. Avoid unnecessary data collection.
9. Maintain auditable security events.
10. Never trust client-side validation alone.

# 3. Authentication Standards

Authentication verifies the identity of an application user.

Requirements:

- Use a well-maintained authentication solution.
- Never store plaintext passwords.
- Use secure password hashing when password authentication is implemented.
- Protect session credentials.
- Enforce session expiration.
- Support secure logout.
- Prevent unauthorized access to protected endpoints.
- Avoid exposing authentication tokens in URLs.
- Apply appropriate login rate limits.
- Never log passwords, access tokens or session secrets.

Authentication and authorization must be implemented as separate controls.

# 4. Authorization and Access Control

Authorization determines which actions an authenticated user may perform.

Niyuvia must support role-based access control.

Proposed roles:

| Role | Responsibility |
|---|---|
| Organization Owner | Organization-level administration |
| Organization Admin | Manage organization settings and users |
| Recruiter | Manage recruitment workflows |
| Hiring Manager | Review assigned candidates and provide feedback |
| Interviewer | Access assigned interviews and submit feedback |

Requirements:

1. Enforce permissions on the backend.
2. Do not rely exclusively on frontend route protection.
3. Verify resource ownership before returning or modifying data.
4. Restrict administrative operations.
5. Apply least-privilege access.
6. Validate permissions on every sensitive operation.
7. Record important permission and administrative changes.

# 5. Multi-Tenant Data Isolation

Organizations must not access one another's private information.

Requirements:

- Associate tenant-owned records with an organization identifier.
- Derive organization context from authenticated server-side identity.
- Never trust an organization identifier supplied only by the client.
- Apply organization filters to database queries.
- Validate ownership during resource updates and deletions.
- Prevent cross-tenant access through direct object references.
- Test tenant isolation independently.

Future production implementation should evaluate PostgreSQL Row-Level Security as an additional defense layer.

Tenant isolation must be demonstrated through tests before commercial deployment.

# 6. Input Validation

All externally supplied data must be validated.

Validation must cover:

- Required fields.
- Data types.
- String lengths.
- Numeric ranges.
- Email formats.
- Phone number formats.
- UUID formats.
- Enumerated values.
- Request payload sizes.
- File metadata.

Backend validation must remain authoritative.

Use Pydantic models for backend request and response validation.

Reject unexpected or malformed input with consistent error responses.

# 7. Output Encoding and Injection Prevention

The application must protect against injection vulnerabilities.

Requirements:

- Use parameterized database queries.
- Avoid dynamically constructed SQL using untrusted input.
- Encode untrusted content before rendering it as HTML.
- Avoid unsafe HTML rendering.
- Validate external URLs.
- Restrict dangerous file-processing behaviour.
- Never execute uploaded document contents as code.
- Avoid shell execution using untrusted input.

The application must also protect against cross-site scripting and cross-site request forgery where applicable to the selected authentication architecture.

# 8. File Upload Security

Resume uploads introduce specific security risks.

Supported initial formats:

- PDF.
- DOCX.

Requirements:

1. Validate file extensions.
2. Validate actual file content and file signatures.
3. Enforce maximum upload sizes.
4. Generate server-controlled storage identifiers.
5. Do not trust original filenames.
6. Store uploaded files outside publicly accessible directories.
7. Restrict download access.
8. Reject unsupported and malformed files.
9. Apply malware scanning before commercial production use.
10. Protect file-processing components from malicious documents.

Uploaded documents must be treated as untrusted input.

# 9. AI-Specific Security

AI functionality introduces additional attack surfaces.

Requirements:

## 9.1 Prompt Injection Protection

- Treat resume text as untrusted data.
- Do not interpret document instructions as system instructions.
- Separate trusted instructions from candidate-provided content.
- Validate model-generated outputs.
- Never allow resume content to authorize tools or privileged actions.

## 9.2 AI Output Validation

- Validate structured outputs against approved schemas.
- Reject invalid output.
- Handle missing and unexpected fields.
- Do not trust AI-generated identifiers or permissions.
- Verify important information against available source evidence.

## 9.3 AI Decision Safety

- AI scores must not independently reject candidates.
- Recruiters must retain decision-making authority.
- Matching explanations must be traceable to evidence.
- Protected characteristics must not be used as ranking criteria.
- AI outputs must be reviewed before consequential recruitment actions.

## 9.4 Provider Security

- Keep provider API keys on the backend.
- Never expose credentials in frontend bundles.
- Avoid sending unnecessary personal information to external providers.
- Document provider data-processing arrangements.
- Apply provider access and retention controls where available.

# 10. API Security

All protected API endpoints must enforce authentication and authorization.

Requirements:

- Validate HTTP methods.
- Validate request payloads.
- Apply request size limits.
- Implement appropriate rate limiting.
- Return consistent error responses.
- Prevent unauthorized resource access.
- Avoid leaking stack traces.
- Protect sensitive administrative endpoints.
- Validate external callback and webhook signatures where applicable.
- Configure CORS using explicit trusted origins.

API security controls must be tested independently of the frontend.

# 11. Secrets Management

Sensitive configuration must never be committed to source control.

Examples:

- Database credentials.
- Authentication secrets.
- AI provider API keys.
- Email credentials.
- Storage credentials.
- Signing secrets.

Requirements:

1. Use environment variables for local configuration.
2. Maintain `.env.example` without actual credentials.
3. Keep `.env` excluded through `.gitignore`.
4. Never hardcode secrets.
5. Rotate exposed credentials immediately.
6. Use a managed secrets service for commercial production where appropriate.
7. Restrict access to secrets.
8. Avoid printing secrets in application logs.

# 12. Database Security

Database access must follow least-privilege principles.

Requirements:

- Use parameterized queries or safe ORM operations.
- Restrict database account permissions.
- Validate tenant ownership.
- Protect database credentials.
- Use encrypted connections where required by the deployment environment.
- Maintain migration history.
- Back up important production data.
- Test recovery procedures.
- Avoid exposing database errors to clients.

Database backups must receive protection equivalent to the original data.

# 13. Privacy and Candidate Data Protection

Niyuvia processes personal information, including candidate resumes and contact details.

Requirements:

- Collect only necessary information.
- Document processing purposes.
- Provide appropriate privacy notices.
- Restrict access to candidate information.
- Establish retention periods.
- Support applicable data access and deletion requests.
- Protect exported candidate information.
- Avoid unnecessary personal data in analytics.
- Document relevant external data processors.
- Maintain appropriate security incident procedures.

Candidate data must not be used for model training by default.

Applicable Indian privacy and data protection requirements must be reviewed before commercial deployment.

# 14. Logging and Security Monitoring

Security-relevant events should be recorded.

Examples:

- Authentication failures.
- Successful administrative actions.
- Permission changes.
- Unauthorized access attempts.
- Sensitive record access where appropriate.
- File upload failures.
- AI provider security errors.
- Configuration and security changes.

Logs must not contain:

- Passwords.
- API keys.
- Session tokens.
- Full resumes.
- Unnecessary candidate personal information.

Logging access and retention must be controlled.

# 15. Dependency Security

Third-party dependencies must be evaluated before adoption.

Requirements:

- Prefer maintained packages.
- Review dependency vulnerabilities.
- Keep lockfiles committed.
- Avoid unnecessary packages.
- Update vulnerable dependencies.
- Review package permissions and licensing.
- Remove unused dependencies.

Dependency security checks should be included in CI when practical.

# 16. Infrastructure and Deployment Security

Before commercial deployment:

- Enforce HTTPS.
- Configure secure HTTP headers.
- Restrict database network access.
- Protect object storage.
- Disable unnecessary services.
- Separate environments.
- Protect deployment credentials.
- Restrict administrative access.
- Establish backup and recovery procedures.
- Monitor security events.

Production deployment must use reviewed configuration rather than development defaults.

# 17. Security Testing

Security validation must include:

| Area | Required Validation |
|---|---|
| Authentication | Unauthorized access prevention |
| Authorization | Role and permission enforcement |
| Tenant isolation | Cross-organization access prevention |
| Input validation | Invalid payload rejection |
| File uploads | Unsupported and malformed file rejection |
| API security | Rate limits and access control |
| Database | Query and ownership protection |
| AI security | Prompt injection and output validation |
| Secrets | No exposed credentials |
| Dependencies | Vulnerability checks |

Critical security defects must block release.

# 18. Security Incident Handling

Security incidents must follow a documented response process.

1. Identify the incident.
2. Assess its scope and severity.
3. Contain affected systems.
4. Preserve relevant evidence.
5. Investigate the root cause.
6. Recover affected services.
7. Rotate compromised credentials where necessary.
8. Evaluate notification obligations.
9. Document corrective actions.

Commercial production must have an approved incident response procedure and designated responsibility.

# 19. MVP Security Priorities

| Control | Priority |
|---|---|
| Authentication enforcement | P0 |
| Backend authorization | P0 |
| Tenant ownership validation | P0 |
| Input validation | P0 |
| Secure secret handling | P0 |
| Safe file upload handling | P0 |
| AI output validation | P0 |
| Prompt injection safeguards | P0 |
| Sensitive logging prevention | P0 |
| Rate limiting | P1 |
| Dependency scanning | P1 |
| Malware scanning | P1 |
| Advanced monitoring | P2 |

# 20. Future Production Security Requirements

Before commercial launch, Niyuvia should undergo:

- Independent security assessment.
- Penetration testing.
- Tenant isolation review.
- Backup restoration testing.
- Incident response exercise.
- Privacy and data-processing review.
- Infrastructure security review.
- Authentication and authorization audit.

These activities are production readiness requirements, not claims of existing certification or compliance.

# 21. Acceptance Criteria

Secure coding practices are considered established when:

1. Authentication is enforced on protected operations.
2. Backend authorization is tested.
3. Tenant isolation is verified.
4. Inputs are validated.
5. File uploads are securely handled.
6. Secrets are excluded from source control.
7. AI outputs are schema-validated.
8. Prompt injection safeguards are tested.
9. Sensitive information is excluded from logs.
10. Security checks are included in the development workflow.

# 22. Document Governance

This document must remain consistent with:

- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- ARCHITECTURE.md
- DATABASE_DESIGN.md
- API_STANDARDS.md
- AI_ARCHITECTURE.md
- AI_PROVIDER_INTEGRATION_SPECIFICATION.md
- AI_OBSERVABILITY_AND_COST_MANAGEMENT.md
- TEST_STRATEGY_AND_QUALITY_PLAN.md
- DEVELOPMENT_WORKFLOW_AND_GIT_STANDARDS.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Security requirements must be reviewed whenever application architecture, data processing, authentication, authorization or external integrations change.

---

**Document Status:** Draft — Pending Review