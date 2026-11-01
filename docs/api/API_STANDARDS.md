# Niyuvia — API Standards and Conventions

**Product:** Niyuvia  
**Document:** API Standards and Conventions  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**API Style:** REST  
**Backend:** Python + FastAPI  
**API Version:** v1  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document establishes the API development standards for Niyuvia.

It defines how the frontend, backend, database and external integrations communicate.

The objective is to ensure:

- Consistent API behavior.
- Predictable request and response formats.
- Secure authentication and authorization.
- Standardized error handling.
- Maintainable API versioning.
- Reliable frontend-backend integration.

# 2. API Architecture

Niyuvia will use RESTful HTTP APIs.

**Base path:**

`/api/v1`

Example:

`/api/v1/jobs`

The API will be implemented using FastAPI and Pydantic.

The backend is responsible for:

- Request validation.
- Authentication.
- Authorization.
- Business logic.
- Database operations.
- AI service orchestration.
- Response serialization.
- Error handling.
- Audit logging.

The frontend must not directly access the database.

# 3. HTTP Methods

| Method | Purpose | Example |
|---|---|---|
| GET | Retrieve resources | GET /jobs |
| POST | Create resources or initiate operations | POST /jobs |
| PUT | Replace a resource | PUT /jobs/{id} |
| PATCH | Partially update a resource | PATCH /jobs/{id} |
| DELETE | Delete a resource where permitted | DELETE /jobs/{id} |

Use HTTP methods according to their intended semantics.

# 4. HTTP Status Codes

| Status | Meaning | Usage |
|---|---|---|
| 200 | OK | Successful retrieval or update |
| 201 | Created | Resource successfully created |
| 202 | Accepted | Asynchronous operation accepted |
| 204 | No Content | Successful operation without response body |
| 400 | Bad Request | Invalid request |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource does not exist or is not accessible |
| 409 | Conflict | Resource or state conflict |
| 413 | Payload Too Large | Request exceeds configured size limit |
| 415 | Unsupported Media Type | Unsupported content type |
| 422 | Unprocessable Entity | Request validation failure |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server error |
| 503 | Service Unavailable | Required service temporarily unavailable |

# 5. Response Standards

All successful JSON responses should follow a consistent envelope.

## 5.1 Successful Response

Example:

    {
      "success": true,
      "data": {
        "id": "uuid",
        "title": "Backend Developer"
      },
      "meta": {
        "request_id": "request-uuid"
      }
    }

## 5.2 Collection Response

    {
      "success": true,
      "data": [
        {
          "id": "uuid",
          "title": "Backend Developer"
        }
      ],
      "meta": {
        "request_id": "request-uuid",
        "pagination": {
          "page": 1,
          "page_size": 20,
          "total": 100,
          "total_pages": 5
        }
      }
    }

## 5.3 Error Response

    {
      "success": false,
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "The submitted information is invalid.",
        "details": []
      },
      "meta": {
        "request_id": "request-uuid"
      }
    }

Error responses must not expose:

- Database credentials.
- Internal stack traces.
- API keys.
- Passwords.
- Sensitive infrastructure details.
- Unnecessary candidate information.

# 6. Naming Conventions

Use:

- Lowercase URL paths.
- Plural resource names.
- Hyphen-separated URL words where needed.
- snake_case JSON properties.
- UUID identifiers.
- Consistent status values.

Examples:

- `/api/v1/jobs`
- `/api/v1/candidates`
- `/api/v1/applications`
- `/api/v1/ai/evaluations`

Avoid inconsistent naming across endpoints.

# 7. Authentication

All internal recruiter and organization-management endpoints must require authentication.

Authentication will be implemented through a securely selected identity provider or authentication mechanism.

Requirements:

- Validate authentication on the backend.
- Verify token signature, issuer, audience and expiry where applicable.
- Never trust user identity supplied in request bodies.
- Never expose authentication secrets in frontend code.
- Apply secure session and token handling.
- Support account deactivation and session revocation.

# 8. Authorization

Authorization must be enforced server-side.

Roles:

- ORGANIZATION_ADMIN
- RECRUITER
- HIRING_MANAGER

Requirements:

1. Verify the authenticated user.
2. Verify organization membership.
3. Verify the user's role.
4. Verify resource ownership.
5. Apply operation-specific permissions.
6. Record sensitive administrative actions.

A hidden frontend button is not an authorization control.

# 9. Organization Isolation

Organization context must be derived from authenticated membership.

The backend must not trust an organization_id supplied by the client as proof of access.

Every organization-owned resource must be checked against the authenticated organization context.

Cross-organization access must be denied unless explicitly authorized by a documented system-level operation.

# 10. Pagination

Collection endpoints must support pagination.

Initial convention:

| Parameter | Default | Maximum |
|---|---:|---:|
| page | 1 | Configured |
| page_size | 20 | 100 |

Example:

`GET /api/v1/jobs?page=1&page_size=20`

Pagination must be deterministic.

Cursor-based pagination may be introduced for large or frequently changing datasets.

# 11. Filtering and Sorting

Collection endpoints may support documented filters.

Example:

`GET /api/v1/jobs?status=OPEN`

Sorting example:

`GET /api/v1/jobs?sort_by=created_at&sort_order=desc`

Requirements:

- Validate filter values.
- Allowlist sortable fields.
- Apply organization authorization before returning results.
- Avoid arbitrary database field selection.

# 12. Validation

Pydantic schemas must validate incoming request data.

Validation must cover:

- Required fields.
- Data types.
- String lengths.
- UUID formats.
- Email formats.
- Numeric ranges.
- Enum values.
- File constraints.
- Business-specific rules.

Database constraints must provide additional protection for critical integrity rules.

# 13. Error Handling

Errors must be consistent and predictable.

Recommended error codes:

| Error Code | Meaning |
|---|---|
| VALIDATION_ERROR | Invalid request data |
| AUTHENTICATION_REQUIRED | Authentication missing |
| INVALID_CREDENTIALS | Invalid authentication credentials |
| PERMISSION_DENIED | Insufficient permissions |
| RESOURCE_NOT_FOUND | Resource unavailable |
| RESOURCE_CONFLICT | Conflicting operation |
| RATE_LIMIT_EXCEEDED | Request limit exceeded |
| AI_PROVIDER_ERROR | AI provider failure |
| FILE_VALIDATION_ERROR | Invalid uploaded file |
| INTERNAL_ERROR | Unexpected backend failure |

Errors must be logged with a request identifier.

# 14. Idempotency

Idempotency must be supported for operations where retries could create duplicate effects.

Priority operations:

- Candidate creation.
- Application creation.
- Resume upload completion.
- AI processing initiation.
- Payment or billing operations if introduced later.
- Notification dispatch.

For applicable POST requests, support an `Idempotency-Key` header.

The backend must define key scope, retention and conflict behavior.

# 15. File Upload APIs

Resume upload endpoints must enforce:

- Allowed file formats.
- Maximum file size.
- File content validation.
- Secure storage.
- Private access.
- Malware scanning or an appropriate security validation process.
- Upload authorization.
- Audit logging.

The backend must not trust the filename or MIME type supplied by the client.

# 16. Asynchronous Operations

Long-running operations should use asynchronous processing where appropriate.

Examples:

- Resume parsing.
- AI candidate evaluation.
- Bulk candidate imports.
- Document processing.

A request may return HTTP 202 with an operation identifier.

Example:

    {
      "success": true,
      "data": {
        "operation_id": "uuid",
        "status": "PENDING"
      },
      "meta": {
        "request_id": "request-uuid"
      }
    }

The frontend can retrieve operation status through a documented status endpoint.

# 17. API Documentation

FastAPI OpenAPI documentation must be maintained.

Development endpoints:

- `/docs`
- `/redoc`
- `/openapi.json`

Interactive API documentation must not expose sensitive production information.

All endpoints must include:

- Description.
- Authentication requirements.
- Request schema.
- Response schema.
- Status codes.
- Error behavior.
- Authorization requirements.

# 18. API Security

The API must implement:

- HTTPS in production.
- Strict CORS configuration.
- Authentication.
- Authorization.
- Input validation.
- Rate limiting.
- Secure file handling.
- Request size limits.
- Secure secret management.
- Appropriate security headers.
- Audit logging.

Never expose AI provider API keys to the browser.

# 19. API Testing

API tests must cover:

- Successful requests.
- Invalid requests.
- Authentication failures.
- Authorization failures.
- Organization isolation.
- Resource not found.
- Duplicate requests.
- Invalid state transitions.
- Rate limits.
- AI provider failures.
- File upload validation.

Use automated tests for critical endpoint behavior.

# 20. API Versioning

The initial API version is:

`/api/v1`

Breaking API changes must not silently alter existing contracts.

Version changes must be documented and reviewed.

Non-breaking additions should preserve compatibility where practical.

# 21. API Governance

This document must remain consistent with:

- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- DATABASE_DESIGN.md
- DATABASE_DATA_DICTIONARY.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- ARCHITECTURE.md

API changes must be reflected in OpenAPI documentation and relevant tests.

---

**Document Status:** Draft — Pending Review

**End of Document**