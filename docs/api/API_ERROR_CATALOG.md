# Niyuvia — API Error Catalog

**Product:** Niyuvia  
**Document:** API Error Catalog  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**API Base Path:** `/api/v1`  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines the standard error codes and error-response conventions used by Niyuvia APIs.

Its objectives are to:

- Standardize backend error responses.
- Provide predictable frontend error handling.
- Improve debugging and observability.
- Prevent accidental exposure of sensitive information.
- Make API failures understandable and actionable.

# 2. Standard Error Response

All API errors must follow this structure:

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

## 2.1 Response fields

| Field | Description |
|---|---|
| success | Always false |
| error.code | Machine-readable error identifier |
| error.message | Safe, human-readable message |
| error.details | Optional validation or contextual details |
| meta.request_id | Unique request correlation identifier |

Error messages must not reveal internal stack traces, credentials, database connection strings or confidential infrastructure details.

# 3. HTTP Status Code Conventions

| HTTP Status | Meaning | Usage |
|---|---|---|
| 400 | Bad Request | Invalid request structure or business operation |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource does not exist or is inaccessible |
| 409 | Conflict | Duplicate or conflicting operation |
| 413 | Payload Too Large | Upload exceeds configured limits |
| 415 | Unsupported Media Type | Unsupported file format |
| 422 | Unprocessable Entity | Field validation failure |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected internal failure |
| 502 | Bad Gateway | Invalid upstream provider response |
| 503 | Service Unavailable | Required dependency unavailable |
| 504 | Gateway Timeout | Upstream operation timed out |

# 4. Authentication Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| AUTHENTICATION_REQUIRED | 401 | Authentication is required |
| INVALID_CREDENTIALS | 401 | Authentication credentials are invalid |
| SESSION_EXPIRED | 401 | Session has expired |
| INVALID_SESSION | 401 | Session is invalid |
| ACCOUNT_DISABLED | 403 | Account access is disabled |

# 5. Authorization Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| PERMISSION_DENIED | 403 | User lacks required permission |
| ROLE_NOT_ALLOWED | 403 | User role cannot perform the operation |
| ORGANIZATION_ACCESS_DENIED | 403 | User cannot access the organization |
| RESOURCE_ACCESS_DENIED | 403 | User cannot access the requested resource |

# 6. Resource Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| RESOURCE_NOT_FOUND | 404 | Requested resource does not exist or is inaccessible |
| JOB_NOT_FOUND | 404 | Job does not exist or is inaccessible |
| CANDIDATE_NOT_FOUND | 404 | Candidate does not exist or is inaccessible |
| APPLICATION_NOT_FOUND | 404 | Application does not exist or is inaccessible |
| RESUME_NOT_FOUND | 404 | Resume does not exist or is inaccessible |
| INTERVIEW_NOT_FOUND | 404 | Interview does not exist or is inaccessible |

Resource-specific errors must not disclose whether another organization's resource exists.

# 7. Validation Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| VALIDATION_ERROR | 422 | Request fields failed validation |
| REQUIRED_FIELD_MISSING | 422 | Required field is missing |
| INVALID_FIELD_VALUE | 422 | Field value is invalid |
| INVALID_DATE_RANGE | 422 | Date range is invalid |
| INVALID_EMAIL_FORMAT | 422 | Email format is invalid |
| INVALID_FILE_FORMAT | 415 | File format is unsupported |

Validation details should identify the relevant field without exposing sensitive input values.

# 8. Conflict Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| DUPLICATE_RESOURCE | 409 | Resource already exists |
| DUPLICATE_APPLICATION | 409 | Equivalent application already exists |
| JOB_STATE_CONFLICT | 409 | Requested operation conflicts with job state |
| APPLICATION_STATE_CONFLICT | 409 | Requested operation conflicts with application state |
| INVALID_STATE_TRANSITION | 409 | Requested workflow transition is not allowed |
| VERSION_CONFLICT | 409 | Resource version changed during the operation |
| IDEMPOTENCY_CONFLICT | 409 | Idempotency key was reused with a different request |

# 9. Resume and File Processing Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| FILE_TOO_LARGE | 413 | File exceeds configured upload limit |
| UNSUPPORTED_FILE_TYPE | 415 | File type is not supported |
| FILE_UPLOAD_FAILED | 500 | File upload failed |
| FILE_NOT_ACCESSIBLE | 403 | User cannot access the file |
| FILE_QUARANTINED | 422 | File was quarantined |
| FILE_PROCESSING_FAILED | 422 | File processing failed |
| EMPTY_DOCUMENT | 422 | Extracted document contains no usable text |

File-processing errors must not expose private storage paths or internal scanning details.

# 10. AI Processing Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| AI_PROVIDER_UNAVAILABLE | 503 | AI provider is unavailable |
| AI_PROVIDER_TIMEOUT | 504 | AI provider timed out |
| AI_PROVIDER_ERROR | 502 | AI provider returned an unsuccessful response |
| AI_INVALID_RESPONSE | 502 | AI output could not be processed |
| AI_SCHEMA_VALIDATION_FAILED | 422 | AI output failed schema validation |
| AI_PROCESSING_FAILED | 500 | AI processing failed unexpectedly |
| AI_RUN_NOT_FOUND | 404 | AI execution does not exist |
| AI_RUN_ALREADY_PROCESSING | 409 | Equivalent processing operation is already running |
| AI_CONFIGURATION_ERROR | 500 | AI provider configuration is invalid |

AI failures must not silently create fabricated candidate information or evaluation results.

A failed AI operation must remain distinguishable from a successful evaluation.

# 11. Interview Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| INTERVIEW_STATE_CONFLICT | 409 | Interview operation conflicts with current state |
| INTERVIEW_TIME_CONFLICT | 409 | Interview scheduling conflict |
| FEEDBACK_NOT_ALLOWED | 403 | User cannot submit feedback |
| FEEDBACK_ALREADY_SUBMITTED | 409 | Feedback already exists and cannot be duplicated |

# 12. Privacy and Data Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| PRIVACY_REQUEST_NOT_FOUND | 404 | Privacy request does not exist |
| PRIVACY_REQUEST_CONFLICT | 409 | Conflicting privacy operation |
| DATA_RETENTION_RESTRICTION | 409 | Deletion is restricted by an applicable retention requirement |
| DATA_EXPORT_FAILED | 500 | Authorized data export failed |
| DATA_DELETION_FAILED | 500 | Data deletion operation failed |

Privacy-related errors must not reveal sensitive candidate information.

# 13. Rate Limiting and Infrastructure Errors

| Error Code | HTTP Status | Description |
|---|---:|---|
| RATE_LIMIT_EXCEEDED | 429 | Request rate limit exceeded |
| REQUEST_TOO_LARGE | 413 | Request exceeds configured size limit |
| DATABASE_UNAVAILABLE | 503 | Database is unavailable |
| STORAGE_UNAVAILABLE | 503 | File storage is unavailable |
| DEPENDENCY_UNAVAILABLE | 503 | Required external dependency is unavailable |
| INTERNAL_SERVER_ERROR | 500 | Unexpected internal failure |
| SERVICE_UNAVAILABLE | 503 | Service is temporarily unavailable |

# 14. Frontend Error Handling

The frontend must handle errors according to their categories.

| Category | Expected Frontend Behaviour |
|---|---|
| Authentication | Request login or session renewal |
| Authorization | Display access-denied message |
| Validation | Display field-level feedback |
| Resource | Display not-found or unavailable state |
| Conflict | Explain why the operation cannot proceed |
| AI processing | Display processing or retry state |
| File processing | Display upload or document-processing feedback |
| Rate limiting | Communicate retry availability |
| Server failure | Display a generic error and allow safe recovery |

The frontend must not display raw server stack traces.

# 15. Retry Policy

Retries must be selective.

- Do not automatically retry invalid requests.
- Do not automatically retry authorization failures.
- Retry temporary network failures only where safe.
- Use bounded retries with exponential backoff for eligible dependency failures.
- Respect provider retry instructions where applicable.
- Use idempotency protection for operations that may be repeated.

AI processing operations must not accidentally create duplicate evaluations because of a client retry.

# 16. Logging and Observability

Backend logs should capture:

- Error code.
- Request ID.
- Timestamp.
- Endpoint.
- HTTP status.
- Relevant organization and user identifiers where permitted.
- Exception classification.
- Dependency failure information where relevant.

Logs must not contain:

- Passwords.
- Authentication tokens.
- API keys.
- Full resume contents.
- Unnecessary personal candidate information.
- Payment or other confidential credentials.

# 17. Testing Requirements

Tests must verify:

1. Correct HTTP status codes.
2. Correct machine-readable error codes.
3. Consistent response structure.
4. Safe error messages.
5. No sensitive information leakage.
6. Correct frontend error handling.
7. Retry behaviour for eligible failures.
8. Correct request ID propagation.

# 18. Document Governance

This document must remain consistent with:

- API_STANDARDS.md
- API_ENDPOINT_CATALOG.md
- FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Any new API error code must be documented here before it becomes part of the stable API contract.

---

**Document Status:** Draft — Pending Review

**End of Document**