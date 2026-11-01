# Niyuvia — Test Strategy and Quality Plan

**Product:** Niyuvia  
**Document:** Test Strategy and Quality Plan  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Owner:** Product and Engineering

---

# 1. Purpose

This document establishes the software testing strategy and quality standards for Niyuvia.

Its objectives are to:

- Prevent defects before release.
- Validate functional requirements.
- Verify frontend and backend integration.
- Protect candidate and organizational data.
- Ensure reliable AI processing.
- Maintain application stability during development.
- Establish repeatable quality assurance procedures.

# 2. Testing Principles

All development must follow these principles:

1. Test functionality, not merely the existence of UI components.
2. Validate both successful and unsuccessful scenarios.
3. Test authorization independently from authentication.
4. Never use real candidate personal information in ordinary test datasets.
5. Test AI outputs against defined schemas and expected behaviour.
6. Automate repeatable tests where practical.
7. Fix critical defects before release.
8. Maintain traceability between requirements and tests.

# 3. Testing Scope

The testing strategy covers:

| Area | Testing Scope |
|---|---|
| Frontend | Components, pages, forms and interactions |
| Backend | Services, business logic and validation |
| Database | Models, constraints and transactions |
| API | Endpoints, responses and error handling |
| Authentication | Login, sessions and authorization |
| Recruitment | Jobs, candidates and applications |
| AI | Parsing, matching and structured outputs |
| Security | Access control, validation and data protection |
| Performance | Response times and resource usage |
| Deployment | Build, configuration and startup validation |

# 4. Testing Levels

## 4.1 Unit Testing

Purpose: Validate individual functions and components independently.

Examples:

- Candidate validation.
- Job requirement validation.
- Matching score calculation.
- Application stage transitions.
- AI output validation.
- Cost calculation.
- Permission checks.

## 4.2 Integration Testing

Purpose: Verify that connected modules work together correctly.

Examples:

- API and database integration.
- Resume upload and text extraction.
- AI provider and response validation.
- Candidate evaluation and application records.
- Authentication and organization authorization.

## 4.3 API Testing

Purpose: Verify API contracts and backend behaviour.

Validate:

- HTTP methods.
- Request validation.
- Response schemas.
- HTTP status codes.
- Authentication requirements.
- Authorization requirements.
- Error responses.
- Pagination.
- Invalid input handling.

## 4.4 End-to-End Testing

Purpose: Validate complete user workflows.

Examples:

1. Recruiter signs in.
2. Recruiter creates a job.
3. Recruiter creates a candidate.
4. Recruiter uploads a resume.
5. System extracts candidate information.
6. Recruiter reviews extracted information.
7. System evaluates candidate-job alignment.
8. Recruiter moves the application through the pipeline.

## 4.5 Security Testing

Purpose: Identify vulnerabilities and unauthorized access.

Validate:

- Authentication enforcement.
- Role-based access control.
- Organization-level data isolation.
- Input validation.
- File upload restrictions.
- Secret management.
- Sensitive information exposure.
- API abuse protections.

# 5. Technology and Testing Tools

The following tools are proposed for the project.

| Layer | Proposed Tool |
|---|---|
| Backend unit tests | Pytest |
| Backend API tests | Pytest and HTTPX |
| Frontend unit tests | Vitest |
| Frontend component tests | React Testing Library |
| Browser automation | Playwright |
| API exploration | FastAPI OpenAPI documentation |
| Static analysis | Ruff and TypeScript |
| Frontend linting | ESLint |
| Dependency checks | Package and dependency auditing |
| CI automation | GitHub Actions |

Tool adoption must remain compatible with the selected implementation stack.

# 6. Functional Test Coverage

The following functional areas require test coverage.

## 6.1 Authentication

- Valid login.
- Invalid login.
- Expired session.
- Unauthorized access.
- Logout.
- Permission enforcement.

## 6.2 Job Management

- Create job.
- View job.
- Update job.
- Archive or close job.
- Validate required fields.
- Reject invalid job data.

## 6.3 Candidate Management

- Create candidate internally.
- View candidate profile.
- Update candidate information.
- Validate email and phone fields.
- Prevent unauthorized access.
- Handle duplicate candidate records according to defined business rules.

## 6.4 Resume Processing

- Accept supported PDF files.
- Accept supported DOCX files.
- Reject unsupported formats.
- Handle corrupted files.
- Handle empty documents.
- Validate extracted information.
- Preserve original uploaded files securely.

## 6.5 Candidate Matching

- Evaluate candidates against approved job requirements.
- Calculate matching dimensions.
- Generate evidence-backed explanations.
- Handle missing information.
- Maintain evaluation version history.
- Prevent automatic rejection based solely on an AI score.

## 6.6 Recruitment Pipeline

- Create applications.
- Change application stages.
- Validate allowed transitions.
- Record application events.
- Preserve stage history.
- Enforce user permissions.

# 7. AI Testing Strategy

AI functionality requires dedicated validation.

## 7.1 Resume Parsing Tests

Validate:

- Structured output correctness.
- Required field handling.
- Missing information.
- Invalid output.
- Hallucinated information.
- Extraction consistency.
- Schema validation.

## 7.2 Candidate Matching Tests

Validate:

- Scoring consistency.
- Correct requirement weighting.
- Evidence traceability.
- Missing-data handling.
- Evaluation reproducibility.
- Version compatibility.

## 7.3 Prompt Safety Tests

Validate:

- Prompt injection in uploaded resumes.
- Malicious instructions inside document text.
- Unexpected model output.
- Sensitive information leakage.
- Unauthorized tool execution.

## 7.4 Mock Provider Tests

During zero-budget development:

- Use deterministic mock responses.
- Avoid external API dependencies.
- Test provider failures.
- Test invalid structured responses.
- Test timeout behaviour.
- Test fallback behaviour.

Mock results must never be represented as real AI model performance.

# 8. Test Data Management

Testing must use synthetic data.

Requirements:

1. Create fictional organizations.
2. Create fictional recruiters.
3. Create fictional candidates.
4. Use synthetic resumes.
5. Avoid real candidate contact information.
6. Keep test data separate from production data.
7. Provide repeatable database setup and cleanup procedures.

# 9. Test Environments

The project should support the following environments.

| Environment | Purpose |
|---|---|
| Local | Individual development |
| Test | Automated and integration testing |
| Staging | Release candidate verification |
| Production | Live commercial deployment |

During the initial zero-budget MVP, local and test environments take priority.

Staging and production environments can be introduced when deployment requirements justify them.

# 10. Defect Severity

| Severity | Description | Expected Action |
|---|---|---|
| Critical | Security breach, data loss or core system failure | Block release |
| High | Major workflow unavailable | Fix before release |
| Medium | Important functionality impaired | Fix or formally defer |
| Low | Minor issue or cosmetic defect | Schedule appropriately |

# 11. Release Quality Gates

A release candidate must satisfy the following conditions:

- Required functional tests pass.
- Critical security tests pass.
- No known unresolved critical defects.
- Backend tests pass.
- Frontend tests pass.
- Build succeeds.
- Database migrations are validated.
- API contracts are consistent.
- AI schema validation works.
- Essential user workflows work.
- Environment configuration is documented.

# 12. Continuous Integration

The planned CI pipeline should perform:

1. Dependency installation.
2. Formatting and lint checks.
3. Static analysis.
4. Backend unit tests.
5. Frontend unit tests.
6. Integration tests.
7. Production build verification.
8. Dependency security checks.

A failed mandatory CI check must prevent the associated release workflow from proceeding.

# 13. Test Reporting

Each test run should record:

- Test run identifier.
- Date and time.
- Commit identifier.
- Environment.
- Test suite.
- Total tests.
- Passed tests.
- Failed tests.
- Skipped tests.
- Execution duration.
- Failure details.

Test reports must not expose secrets or candidate personal information.

# 14. MVP Testing Priorities

| Testing Capability | Priority |
|---|---|
| Backend unit tests | P0 |
| API integration tests | P0 |
| Authentication and authorization tests | P0 |
| Database tests | P0 |
| Resume processing tests | P0 |
| AI mock provider tests | P0 |
| Candidate matching tests | P0 |
| Essential Playwright workflows | P1 |
| Automated security scanning | P1 |
| Performance testing | P1 |
| Advanced load testing | P2 |

# 15. Future Quality Improvements

Future enhancements may include:

- Test coverage reporting.
- Mutation testing.
- Automated accessibility testing.
- Load and stress testing.
- Regression test automation.
- Security penetration testing.
- AI evaluation benchmarks.
- Automated deployment verification.

# 16. Acceptance Criteria

The testing framework is considered established when:

1. Tests can run locally using documented commands.
2. Core backend functionality has automated tests.
3. Essential frontend workflows have automated tests.
4. API integration tests validate expected contracts.
5. Database constraints are tested.
6. AI mock workflows are deterministic.
7. Security-critical authorization behaviour is tested.
8. CI executes mandatory quality checks.
9. Test results are inspectable.
10. Release quality gates are documented.

# 17. Document Governance

This document must remain consistent with:

- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- USER_STORIES.md
- ARCHITECTURE.md
- DATABASE_DESIGN.md
- API_STANDARDS.md
- AI_ARCHITECTURE.md
- AI_EVALUATION_AND_TESTING.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Changes to testing standards must be reviewed when the architecture, requirements or technology stack changes.

---

**Document Status:** Draft — Pending Review