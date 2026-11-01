# Niyuvia — API Endpoint Catalog

**Product:** Niyuvia  
**Document:** API Endpoint Catalog  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**API Style:** REST  
**Base Path:** `/api/v1`  
**Backend:** FastAPI  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines the planned API endpoints for the Niyuvia MVP.

It provides a common contract for frontend and backend development.

Each endpoint must follow the conventions defined in:

- API_STANDARDS.md
- FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- DATABASE_DESIGN.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

# 2. API Scope

The initial MVP supports internal recruitment operations.

Included:

- Recruiter authentication.
- Organization management.
- Job management.
- Internal candidate management.
- Resume upload and processing.
- Application management.
- AI-assisted resume parsing.
- AI-assisted candidate evaluation.
- Interview management.
- Dashboard analytics.
- Audit and privacy operations.

Excluded from the initial MVP:

- Candidate login accounts.
- Public job listings.
- Public candidate registration.
- Public application submission.
- Automated external candidate sourcing.
- Paid subscription management.

# 3. Authentication and Authorization

All internal endpoints require an authenticated user unless explicitly documented otherwise.

Roles:

| Role | Description |
|---|---|
| ORGANIZATION_ADMIN | Organization administration |
| RECRUITER | Recruitment operations |
| HIRING_MANAGER | Job and interview evaluation |

Authorization must be checked by the backend.

Organization context must be derived from authenticated membership, not trusted from arbitrary client input.

# 4. Authentication Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/auth/me` | Retrieve current user profile | Authenticated |
| POST | `/auth/logout` | End current session | Authenticated |
| POST | `/auth/refresh` | Refresh authentication session, if supported by selected auth mechanism | Authenticated |

Authentication provider selection remains an implementation decision.

# 5. Organization Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/organizations/current` | Retrieve current organization | Organization member |
| PATCH | `/organizations/current` | Update organization details | ORGANIZATION_ADMIN |
| GET | `/organizations/current/members` | List organization members | ORGANIZATION_ADMIN |
| GET | `/organizations/current/members/{member_id}` | Retrieve member details | ORGANIZATION_ADMIN |
| PATCH | `/organizations/current/members/{member_id}` | Update member role or status | ORGANIZATION_ADMIN |

# 6. Department Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/departments` | List departments | Organization member |
| POST | `/departments` | Create department | ORGANIZATION_ADMIN |
| GET | `/departments/{department_id}` | Retrieve department | Organization member |
| PATCH | `/departments/{department_id}` | Update department | ORGANIZATION_ADMIN |
| DELETE | `/departments/{department_id}` | Delete department where permitted | ORGANIZATION_ADMIN |

# 7. Job Management Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/jobs` | List organization jobs | Organization member |
| POST | `/jobs` | Create job | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/jobs/{job_id}` | Retrieve job details | Organization member |
| PATCH | `/jobs/{job_id}` | Update job details | RECRUITER, ORGANIZATION_ADMIN |
| DELETE | `/jobs/{job_id}` | Delete eligible draft job | ORGANIZATION_ADMIN |
| POST | `/jobs/{job_id}/publish` | Open job for internal recruitment | RECRUITER, ORGANIZATION_ADMIN |
| POST | `/jobs/{job_id}/pause` | Pause job | RECRUITER, ORGANIZATION_ADMIN |
| POST | `/jobs/{job_id}/close` | Close job | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/jobs/{job_id}/requirements` | List job requirements | Organization member |
| POST | `/jobs/{job_id}/requirements` | Add job requirement | RECRUITER, ORGANIZATION_ADMIN |
| PATCH | `/jobs/{job_id}/requirements/{requirement_id}` | Update job requirement | RECRUITER, ORGANIZATION_ADMIN |
| DELETE | `/jobs/{job_id}/requirements/{requirement_id}` | Remove eligible requirement | RECRUITER, ORGANIZATION_ADMIN |

Job requirement changes must follow the approved versioning rules.

# 8. Candidate Management Endpoints

Candidates are managed internally by authorized organization members.

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/candidates` | List candidates | Organization member |
| POST | `/candidates` | Create candidate | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/candidates/{candidate_id}` | Retrieve candidate profile | Organization member |
| PATCH | `/candidates/{candidate_id}` | Update candidate profile | RECRUITER, ORGANIZATION_ADMIN |
| DELETE | `/candidates/{candidate_id}` | Process eligible deletion request | ORGANIZATION_ADMIN |
| GET | `/candidates/{candidate_id}/applications` | List candidate applications | Organization member |
| GET | `/candidates/{candidate_id}/resumes` | List candidate resumes | Organization member |

Candidate endpoints must enforce organization ownership.

# 9. Resume Management Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| POST | `/resumes/upload` | Upload candidate resume | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/resumes/{resume_id}` | Retrieve resume metadata | Organization member |
| GET | `/resumes/{resume_id}/download` | Obtain authorized resume download | Organization member |
| DELETE | `/resumes/{resume_id}` | Request eligible resume deletion | ORGANIZATION_ADMIN |
| GET | `/resumes/{resume_id}/status` | Retrieve processing status | Organization member |

Supported file formats and upload limits must be configured and enforced by the backend.

Resume files must remain private.

# 10. Application Management Endpoints

Applications are created internally in the initial MVP.

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/applications` | List applications | Organization member |
| POST | `/applications` | Create internal application | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/applications/{application_id}` | Retrieve application details | Organization member |
| PATCH | `/applications/{application_id}` | Update permitted application fields | RECRUITER, ORGANIZATION_ADMIN |
| POST | `/applications/{application_id}/stage` | Change recruitment stage | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/applications/{application_id}/events` | Retrieve application history | Organization member |
| GET | `/applications/{application_id}/evaluations` | Retrieve AI evaluation history | Organization member |
| GET | `/applications/{application_id}/interviews` | Retrieve associated interviews | Organization member |

Every stage change must follow the application state-transition rules.

Every successful stage change must create an application event.

# 11. AI Processing Endpoints

AI operations must be initiated and controlled by the backend.

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| POST | `/ai/resumes/{resume_id}/parse` | Initiate resume parsing | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/ai/runs/{run_id}` | Retrieve AI execution status | Organization member |
| POST | `/ai/applications/{application_id}/evaluate` | Initiate candidate evaluation | RECRUITER, ORGANIZATION_ADMIN |
| GET | `/ai/evaluations/{evaluation_id}` | Retrieve evaluation details | Organization member |
| POST | `/ai/interviews/generate-questions` | Generate structured interview questions | RECRUITER, HIRING_MANAGER |

AI processing responses must include execution identifiers and appropriate processing status.

AI outputs must be validated before being stored or displayed.

AI evaluations must include supporting evidence and must not independently reject candidates.

# 12. Interview Management Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/interviews` | List interviews | Organization member |
| POST | `/interviews` | Schedule interview | RECRUITER, HIRING_MANAGER |
| GET | `/interviews/{interview_id}` | Retrieve interview details | Organization member |
| PATCH | `/interviews/{interview_id}` | Update interview details | RECRUITER, HIRING_MANAGER |
| POST | `/interviews/{interview_id}/cancel` | Cancel interview | RECRUITER, HIRING_MANAGER |
| GET | `/interviews/{interview_id}/feedback` | Retrieve interview feedback | Authorized member |
| POST | `/interviews/{interview_id}/feedback` | Submit interview feedback | HIRING_MANAGER, RECRUITER |

Submitted feedback must be attributable to its author and must follow the feedback update policy.

# 13. Dashboard and Analytics Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/analytics/overview` | Retrieve dashboard summary | Organization member |
| GET | `/analytics/recruitment-funnel` | Retrieve application stage distribution | Organization member |
| GET | `/analytics/jobs` | Retrieve job-related statistics | Organization member |
| GET | `/analytics/applications` | Retrieve application statistics | Organization member |
| GET | `/analytics/interviews` | Retrieve interview statistics | Organization member |

Analytics must be calculated from authorized organization data.

Metrics must have documented definitions and must not imply unsupported business outcomes.

# 14. Audit Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/audit-logs` | Retrieve organization audit history | ORGANIZATION_ADMIN |
| GET | `/audit-logs/{log_id}` | Retrieve individual audit event | ORGANIZATION_ADMIN |

Audit records must be protected from unauthorized modification.

Sensitive information must not be exposed through audit responses.

# 15. Privacy Endpoints

| Method | Endpoint | Purpose | Access |
|---|---|---|---|
| GET | `/privacy/records` | Retrieve authorized privacy records | ORGANIZATION_ADMIN |
| POST | `/privacy/requests` | Record a privacy request | ORGANIZATION_ADMIN |
| GET | `/privacy/requests/{request_id}` | Retrieve privacy request status | ORGANIZATION_ADMIN |
| PATCH | `/privacy/requests/{request_id}` | Update privacy request processing status | ORGANIZATION_ADMIN |

Privacy operations must follow the approved privacy and retention policies.

# 16. Common Query Parameters

Where applicable, collection endpoints should support:

| Parameter | Purpose |
|---|---|
| page | Page number |
| page_size | Number of records per page |
| search | Text search |
| sort_by | Allowlisted sorting field |
| sort_order | asc or desc |
| created_from | Start date filter |
| created_to | End date filter |
| status | Resource status filter |

Unsupported filters must return a validation error.

# 17. Standard Response Format

Successful response:

    {
      "success": true,
      "data": {},
      "meta": {
        "request_id": "request-uuid"
      }
    }

Error response:

    {
      "success": false,
      "error": {
        "code": "RESOURCE_NOT_FOUND",
        "message": "The requested resource was not found.",
        "details": []
      },
      "meta": {
        "request_id": "request-uuid"
      }
    }

# 18. API Security Requirements

All endpoints must enforce applicable security controls:

- Authentication.
- Role-based authorization.
- Organization isolation.
- Input validation.
- Rate limiting.
- Request size limits.
- Secure file access.
- Audit logging.
- Sensitive data protection.

Public candidate registration and public application endpoints must not be introduced into the initial MVP.

# 19. API Testing Requirements

Every implemented endpoint must have tests for:

1. Successful execution.
2. Invalid request payload.
3. Missing authentication.
4. Insufficient permissions.
5. Cross-organization access attempts.
6. Missing resources.
7. Invalid resource state.
8. Database failures where relevant.
9. External service failures where relevant.

Critical security and business-rule tests must be automated.

# 20. Implementation Governance

This catalog defines planned endpoints, not proof of implementation.

Before an endpoint is implemented, its request schema, response schema, permissions, status codes and business rules must be defined.

Any endpoint changes must be reflected in:

- API_STANDARDS.md
- FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- DATABASE_DESIGN.md
- API tests
- OpenAPI documentation

---

**Document Status:** Draft — Pending Review

**End of Document**