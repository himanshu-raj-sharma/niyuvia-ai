# Niyuvia — Functional Requirements Specification (FRS)

**Product:** Niyuvia  
**Document Type:** Functional Requirements Specification  
**Version:** 1.1  
**Status:** Draft — Pending Consolidated Review  
**Date:** 31 October 2026  
**Related Documents:** PRD.md, PRODUCT_VISION.md, TARGET_USERS_AND_PERSONAS.md, MVP_SCOPE_AND_RELEASE_PLAN.md, BUSINESS_RULES.md

---

# 1. Introduction

## 1.1 Purpose

This document defines the functional behaviour expected from Niyuvia.

It translates product requirements into specific, identifiable and testable system requirements.

It serves as a reference for:

- Frontend development.
- Backend development.
- Database design.
- API implementation.
- AI integration.
- Quality assurance.
- Acceptance testing.

## 1.2 Scope

This specification covers the initial Niyuvia portfolio MVP.

Niyuvia V1 is an internal recruiter-facing Applicant Tracking System (ATS).

The primary application users are:

1. Organization Administrator.
2. Recruiter.
3. Hiring Manager.

Candidates are external data subjects whose records are created and managed by authorized organization users.

Candidates are not authenticated application users in the initial MVP.

The primary functional modules are:

1. Authentication and authorization.
2. Organization management.
3. Job management.
4. Internal candidate management.
5. Resume management.
6. AI-assisted resume parsing.
7. Job requirement analysis.
8. Candidate-job matching.
9. Application management.
10. Recruitment pipeline.
11. Interview management.
12. Dashboard and analytics.
13. Search and filtering.
14. Notifications and processing status.
15. Audit and activity tracking.
16. Privacy and candidate data operations.
17. API behaviour and error handling.

Advanced commercial features and candidate-facing functionality are documented as future scope and are not mandatory for the initial MVP.

## 1.3 Requirement Priority

| Priority | Meaning |
|---|---|
| P0 | Critical for MVP |
| P1 | Important for MVP |
| P2 | Optional or future enhancement |

A requirement must not be considered implemented merely because its user interface exists. Its expected backend behaviour, validation, authorization and persistence must also work.

## 1.4 MVP Boundary

The initial MVP supports internal recruitment operations.

Included:

- Organization user authentication.
- Job creation and management.
- Internal candidate creation and management.
- Resume upload and processing.
- AI-assisted resume parsing.
- Job requirement analysis.
- Candidate-job matching.
- Internal application management.
- Recruitment pipeline management.
- Basic interview management.
- Basic recruitment analytics.

Excluded:

- Candidate login.
- Candidate self-registration.
- Public job listings.
- Public application submission.
- Candidate-facing dashboards.
- Candidate self-service application tracking.

All initial candidate records and applications must be created through authorized internal workflows.

---

# 2. Authentication and Authorization

## 2.1 Authentication

### FR-AUTH-001 — User Sign In

**Priority:** P0

The system shall allow registered organization users to sign in using the configured authentication mechanism.

Supported application roles:

- Organization Administrator.
- Recruiter.
- Hiring Manager.

Acceptance criteria:

- Valid credentials establish an authenticated session.
- Invalid credentials are rejected.
- Error responses do not reveal sensitive authentication information.
- Successful authentication provides the user with the appropriate application access.
- Candidate accounts are not supported in the initial MVP.

### FR-AUTH-002 — User Sign Out

**Priority:** P0

The system shall allow authenticated users to sign out.

Acceptance criteria:

- The authenticated session is terminated or invalidated.
- Protected resources cannot be accessed using an invalidated session.
- The frontend updates its authentication state.

### FR-AUTH-003 — Authentication State

**Priority:** P0

The frontend shall maintain an accurate representation of authentication state.

Acceptance criteria:

- Authenticated users can access permitted pages.
- Unauthenticated users are redirected or denied access.
- Expired sessions are handled appropriately.
- Authentication state is not trusted as a substitute for backend authorization.

## 2.2 Authorization

### FR-AUTH-004 — Role-Based Permissions

**Priority:** P0

The system shall enforce role-based access controls.

Initial roles:

- Organization Administrator.
- Recruiter.
- Hiring Manager.

Acceptance criteria:

- Each protected operation has a defined permission.
- Backend authorization is enforced independently of frontend visibility.
- Unauthorized operations return an appropriate error.
- Organization boundaries are enforced.
- Permissions are documented in the role-permission matrix.

### FR-AUTH-005 — Organization Context

**Priority:** P0

Organization-owned records shall be associated with the authenticated user's authorized organization.

Acceptance criteria:

- Organization identity is derived from trusted authentication context.
- Clients cannot access another organization's records by changing request parameters.
- Organization ownership is validated during reads and writes.
- The backend rejects records that do not belong to the authenticated organization.

---

# 3. Organization Management

### FR-ORG-001 — Organization Profile

**Priority:** P1

Authorized administrators shall be able to view and update basic organization information.

Initial fields:

- Organization name.
- Organization description.
- Website.
- Location.
- Contact information.

Acceptance criteria:

- Organization information is associated with the correct organization.
- Only authorized users can update organization details.
- Invalid information is rejected.
- Changes are persisted correctly.

### FR-ORG-002 — Organization Membership

**Priority:** P2

The system should support organization membership management.

Potential functionality:

- Invite members.
- Assign roles.
- Update permissions.
- Remove members.

For the initial MVP, membership management may use a simplified setup.

A development seed process or administrator-controlled setup may be used initially.

External invitation emails are not mandatory for the zero-budget MVP.

---

# 4. Job Management

## 4.1 Job Creation

### FR-JOB-001 — Create Job

**Priority:** P0

Authorized recruiters shall be able to create a job opening.

Required fields:

- Job title.
- Job description.
- Employment type.
- Job location or remote designation.

Additional fields:

- Department.
- Required skills.
- Preferred skills.
- Minimum experience.
- Maximum experience.
- Work mode.
- Salary range, if provided.
- Job status.

Acceptance criteria:

- Required fields are validated.
- Invalid data is rejected.
- The job is associated with the correct organization.
- A successful operation returns the created job record.
- The initial status is Draft unless an explicitly permitted status is selected.
- Job creation does not automatically publish a public job listing.

## 4.2 Job Listing

### FR-JOB-002 — List Jobs

**Priority:** P0

Authorized users shall be able to view jobs belonging to their organization.

Acceptance criteria:

- Jobs are restricted by organization.
- Results support pagination.
- Users can filter by status.
- Users can search by job title.
- Only internal application users can access the job management interface.

## 4.3 Job Details

### FR-JOB-003 — View Job

**Priority:** P0

Authorized users shall be able to inspect complete job details.

Acceptance criteria:

- Job information is displayed accurately.
- Requirements and job status are available.
- Associated application information is accessible to authorized users.
- Cross-organization records are not exposed.

### FR-JOB-004 — Update Job

**Priority:** P0

Authorized users shall be able to update permitted job fields.

Acceptance criteria:

- Updates are validated.
- Unauthorized users cannot modify jobs.
- Changes are persisted correctly.
- Changes to approved job requirements can be traced.

### FR-JOB-005 — Job Status Management

**Priority:** P0

Jobs shall support:

- Draft.
- Open.
- Paused.
- Closed.

Status transitions must be validated.

Acceptance criteria:

- Only permitted transitions are accepted.
- Updated status is persisted.
- The current status is displayed correctly.
- Historical recruitment records remain associated with the job.

### FR-JOB-006 — Delete or Archive Job

**Priority:** P1

The system should support safe job archival or deletion.

Acceptance criteria:

- Archival is preferred when historical applications exist.
- Applications and historical recruitment records must not be accidentally destroyed.
- Destructive operations require authorization.
- Relevant deletion and archival actions should be traceable.

---

# 5. Internal Candidate Management

## 5.1 Candidate Creation

### FR-CAND-001 — Create Candidate

**Priority:** P0

Authorized recruiters shall be able to create candidate records internally.

Initial fields:

- Full name.
- Email.
- Phone.
- Location.
- Professional summary.
- Skills.
- Experience.
- Education.

Acceptance criteria:

- Required fields are validated.
- Email format is validated where an email is provided.
- Candidate records are associated with the correct organization.
- Sensitive optional fields are not required unnecessarily.
- Candidate creation is restricted to authorized organization users.
- Candidate records are not created through public self-registration.

### FR-CAND-002 — Candidate Listing

**Priority:** P0

Recruiters shall be able to list candidates belonging to their organization.

The listing should support:

- Search by name.
- Search by email.
- Filtering by relevant skills.
- Pagination.

Acceptance criteria:

- Results are organization-scoped.
- Search results reflect stored candidate information.
- Unauthorized users cannot access candidate records.
- Empty results are handled clearly.

### FR-CAND-003 — Candidate Profile

**Priority:** P0

The system shall provide a detailed candidate profile.

The profile should contain:

- Personal information required for recruitment.
- Professional summary.
- Skills.
- Experience.
- Education.
- Projects.
- Associated resumes.
- Associated applications.
- Relevant AI extraction results.
- Matching evaluation history where available.

### FR-CAND-004 — Candidate Update

**Priority:** P0

Authorized users shall be able to correct or update candidate information.

Acceptance criteria:

- Updated fields are validated.
- Only authorized users can modify candidate records.
- Changes are persisted.
- Important changes can be traced.
- AI-generated information does not silently overwrite recruiter-approved information.

### FR-CAND-005 — Duplicate Candidate Handling

**Priority:** P1

The system should identify potential duplicate candidate records.

Acceptance criteria:

- Matching email addresses can trigger a duplicate warning.
- Recruiters can review possible duplicates.
- The system must not silently merge different candidate records.
- Duplicate detection is organization-scoped.
- Potential duplicates do not automatically result in record deletion.

## 5.2 Candidate Access Boundary

The initial MVP must not provide candidate-facing account functionality.

The following are excluded:

- Candidate registration.
- Candidate sign in.
- Candidate password management.
- Candidate dashboards.
- Candidate self-service profile editing.
- Candidate application tracking.
- Public candidate application submission.

Candidate records are maintained by authorized organization users.

---

# 6. Resume Management

## 6.1 Resume Upload

### FR-RES-001 — Upload Resume

**Priority:** P0

Authorized recruiters shall be able to upload candidate resumes.

Supported formats:

- PDF.
- DOCX.

Acceptance criteria:

- File type is validated.
- File size is restricted by configuration.
- Invalid files are rejected.
- Original filenames are handled safely.
- Uploaded documents are associated with the correct candidate.
- Storage references are persisted.
- Upload access is restricted to authorized users.

### FR-RES-002 — Secure File Storage

**Priority:** P0

Uploaded resumes shall be stored outside publicly accessible application directories.

Acceptance criteria:

- Resume files require authorized access.
- File access is restricted to permitted users.
- Storage paths are not directly exposed as public download links.
- Files cannot be accessed by changing client-side identifiers alone.
- Local development storage must follow the same access-control principles.

### FR-RES-003 — Extract Resume Text

**Priority:** P0

The backend shall extract readable text from supported document formats.

Acceptance criteria:

- PDF files are processed using a supported extraction library.
- DOCX files are processed using a supported extraction library.
- Empty or unreadable documents are handled.
- Extraction failures are recorded.
- Original files remain preserved.
- Extracted text is associated with the correct resume.

### FR-RES-004 — Resume Processing Status

**Priority:** P0

The system shall maintain a processing status.

Supported states:

- Pending.
- Processing.
- Completed.
- Failed.

Acceptance criteria:

- Status changes are persisted.
- Users can identify processing progress.
- Failures are communicated clearly.
- Failed processing does not silently modify candidate information.

### FR-RES-005 — Resume Versioning

**Priority:** P1

The system should preserve associations between uploaded resume versions and their extraction results.

Acceptance criteria:

- Each uploaded version can be identified.
- Previous resume records are not silently overwritten.
- Extraction results reference the corresponding resume version.

### FR-RES-006 — Resume Deletion

**Priority:** P1

Authorized users should be able to request resume deletion according to the configured retention policy.

Acceptance criteria:

- Deletion requests are authorized.
- Related extracted information and storage references are handled consistently.
- Relevant audit records are retained where appropriate.
- Deletion behaviour follows the documented retention rules.

---

# 7. AI-Assisted Resume Parsing

### FR-AI-001 — Structured Resume Extraction

**Priority:** P0

The system shall convert extracted resume text into a structured candidate information representation.

Expected information:

- Candidate name.
- Contact information.
- Professional summary.
- Skills.
- Work experience.
- Education.
- Projects.
- Certifications.

Acceptance criteria:

- Output follows the defined structured schema.
- Missing information is represented explicitly.
- Extracted values retain appropriate source references where feasible.
- The system does not invent candidate information.

### FR-AI-002 — Schema Validation

**Priority:** P0

AI-generated structured output shall be validated against a defined schema before it is accepted.

Acceptance criteria:

- Invalid structures are rejected.
- Missing fields are represented appropriately.
- Data types are validated.
- Malformed output does not silently overwrite existing candidate information.
- Validation errors are recorded.

### FR-AI-003 — Human Review

**Priority:** P0

Recruiters shall be able to inspect and correct extracted information.

Acceptance criteria:

- Parsed information is presented before final acceptance.
- Recruiters can edit incorrect fields.
- Approved information is stored in the candidate record.
- Unapproved extraction results remain distinguishable from verified candidate information.

### FR-AI-004 — Processing Metadata

**Priority:** P1

The system should retain relevant processing metadata:

- Processing timestamp.
- Provider or processing mechanism.
- Model identifier where applicable.
- Prompt version where applicable.
- Processing status.
- Validation outcome.

### FR-AI-005 — Untrusted Resume Content

**Priority:** P0

Resume content must be treated as untrusted input.

Acceptance criteria:

- Instructions embedded in resumes cannot override system instructions.
- Resume content cannot authorize tools or operations.
- External document text cannot trigger unauthorized actions.
- Parsing output is validated before being used by application services.

### FR-AI-006 — Mock AI Provider

**Priority:** P0

The system shall support a mock AI provider for zero-budget development and automated testing.

Acceptance criteria:

- The mock provider works without external API credentials.
- The provider produces predictable structured results.
- Test cases can simulate successful processing.
- Test cases can simulate invalid output and provider failures.
- Provider-specific implementation details are isolated behind an abstraction.
- Switching to an external provider does not require rewriting recruitment business logic.

External AI API usage is optional for the initial MVP.

---

# 8. Job Requirement Analysis

### FR-JOB-AI-001 — Structured Job Requirements

**Priority:** P0

The system shall represent job requirements in a structured format.

Expected categories:

- Required skills.
- Preferred skills.
- Experience requirements.
- Responsibilities.
- Relevant qualifications.

Acceptance criteria:

- Required and preferred skills are distinguishable.
- Structured requirements are associated with the correct job.
- Manual requirement entry is supported.
- Invalid requirement structures are rejected.

### FR-JOB-AI-002 — Recruiter Review

**Priority:** P0

Recruiters must be able to review and correct extracted job requirements before candidate matching.

Acceptance criteria:

- Proposed requirements are displayed for review.
- Recruiters can correct inaccurate requirements.
- Approved requirements are stored.
- Matching uses approved requirements.

### FR-JOB-AI-003 — Requirement Versioning

**Priority:** P1

Changes to approved job requirements should be traceable because they may affect candidate matching results.

Acceptance criteria:

- Requirement changes can be identified.
- Previous evaluations remain associated with their original requirement versions.
- Re-evaluation can use the latest approved requirements.

---

# 9. Candidate-Job Matching

### FR-MATCH-001 — Matching Calculation

**Priority:** P0

The system shall calculate a candidate-job alignment score using documented criteria.

Initial proposed weights:

| Dimension | Weight |
|---|---:|
| Required skills | 45% |
| Preferred skills | 15% |
| Relevant experience | 15% |
| Projects and responsibilities | 25% |
| Total | 100% |

Acceptance criteria:

- The scoring method is deterministic for a given input and scoring version.
- Weights are validated.
- The total configured weight equals 100%.
- The scoring calculation can be inspected and tested.
- Missing information is handled separately from negative evidence.
- The score represents job alignment, not probability of hiring success.

### FR-MATCH-002 — Evidence Collection

**Priority:** P0

The matching engine shall associate assessment results with available candidate evidence.

Acceptance criteria:

- Evidence is linked to candidate information.
- Evidence can be inspected by authorized recruiters.
- Unsupported claims are not generated.
- Missing evidence is identified separately.

### FR-MATCH-003 — Missing Information

**Priority:** P0

Missing information must be represented separately from negative evidence.

Acceptance criteria:

- Missing information is explicitly identified.
- Absence of resume evidence does not automatically indicate lack of capability.
- Incomplete records can still be reviewed.
- The scoring implementation documents how missing data affects calculations.

### FR-MATCH-004 — Match Explanation

**Priority:** P0

Recruiters shall be able to inspect:

- Overall alignment score.
- Dimension-level scores.
- Supporting evidence.
- Missing information.
- Verification requirements.

### FR-MATCH-005 — Evaluation Versioning

**Priority:** P1

The system should retain the matching configuration and evaluation version.

Acceptance criteria:

- Evaluation records identify the scoring version.
- Requirement versions can be identified.
- Historical results are not silently overwritten.

### FR-MATCH-006 — Human Decision

**Priority:** P0

Matching results shall not automatically reject candidates or independently determine hiring outcomes.

The recruiter remains responsible for decisions.

### FR-MATCH-007 — Re-Evaluation

**Priority:** P1

Authorized users should be able to request a new assessment when approved candidate information or job requirements change.

Acceptance criteria:

- Re-evaluation uses current approved information.
- Previous evaluations remain identifiable.
- The reason for re-evaluation can be recorded.

---

# 10. Application Management

### FR-APP-001 — Create Application

**Priority:** P0

The system shall associate a candidate with a job through an application record.

Applications must be created through authorized internal organization workflows.

Acceptance criteria:

- The candidate exists.
- The job exists.
- Both belong to the authorized organization.
- The user has permission to create the application.
- Duplicate active applications for the same candidate and job are handled according to defined rules.
- Application creation is not dependent on public application submission.

### FR-APP-002 — Application Details

**Priority:** P0

Authorized users shall be able to view an application's details.

The application detail should include:

- Candidate information.
- Job information.
- Current recruitment stage.
- Relevant matching evaluation.
- Interview information where available.
- Application history where available.

### FR-APP-003 — Application Listing

**Priority:** P0

The system shall provide organization-scoped application listings.

Acceptance criteria:

- Applications are restricted by organization.
- Pagination is supported.
- Relevant filtering is available.
- Unauthorized records are not returned.

### FR-APP-004 — Application History

**Priority:** P1

Important application changes should be recorded as historical events.

Acceptance criteria:

- Stage changes can be recorded.
- Relevant acting user is identified.
- Timestamps are retained.
- Historical records are associated with the correct application.

## 10.1 Internal Application Creation Boundary

The initial MVP supports:

- Recruiter-created applications.
- Applications created from internal candidate records.
- Applications associated with internal job records.

The initial MVP excludes:

- Public application forms.
- Candidate self-registration.
- Candidate-submitted applications.
- Public application APIs.
- Candidate self-service application tracking.

---

# 11. Recruitment Pipeline

### FR-PIPE-001 — Pipeline Stages

**Priority:** P0

Applications shall support these stages:

1. APPLIED
2. UNDER_REVIEW
3. SHORTLISTED
4. INTERVIEW_SCHEDULED
5. INTERVIEW_COMPLETED
6. SELECTED
7. REJECTED
8. WITHDRAWN
9. HIRED

### FR-PIPE-002 — Update Application Stage

**Priority:** P0

Authorized recruiters shall be able to update application stages.

Acceptance criteria:

- Permitted transitions are validated.
- Invalid transitions are rejected.
- Updates are associated with the correct application.
- The change is persisted.
- The acting user is recorded where supported.

### FR-PIPE-003 — Stage History

**Priority:** P1

The system should record:

- Previous stage.
- New stage.
- Change timestamp.
- Acting user.
- Optional reason or note.

### FR-PIPE-004 — Pipeline Filtering

**Priority:** P0

Recruiters shall be able to filter applications by:

- Job.
- Stage.
- Candidate.
- Relevant application information.

Acceptance criteria:

- Filters operate on authorized organization records.
- Multiple filters can be combined where supported.
- Results are paginated.

---

# 12. Interview Management

### FR-INT-001 — Create Interview

**Priority:** P1

Authorized users shall be able to schedule an interview for an application.

Fields:

- Application.
- Interview date.
- Interview time.
- Interview type.
- Interviewer.
- Meeting details where applicable.
- Status.

Acceptance criteria:

- The application exists and belongs to the authorized organization.
- Interview details are validated.
- The interview is associated with the correct application.
- Unauthorized users cannot create interview records.

### FR-INT-002 — Interview Listing

**Priority:** P1

Authorized users shall be able to view scheduled interviews.

### FR-INT-003 — Update Interview

**Priority:** P1

Authorized users shall be able to reschedule or update interview records.

Acceptance criteria:

- Updates are validated.
- Historical details can be retained where appropriate.
- Unauthorized changes are rejected.

### FR-INT-004 — Interview Feedback

**Priority:** P1

Authorized interviewers shall be able to submit structured feedback.

Possible feedback fields:

- Technical assessment.
- Communication assessment.
- Role-specific observations.
- Strengths.
- Areas requiring further evaluation.
- Overall recommendation.
- Interview notes.

Acceptance criteria:

- Feedback is associated with the correct interview.
- The acting interviewer is identified.
- Feedback access is restricted.
- Feedback cannot silently alter the application stage without an authorized operation.

### FR-INT-005 — Interview Question Assistance

**Priority:** P2

The system may generate suggested interview questions based on approved job requirements.

Generated questions must be reviewed by the interviewer.

External calendar integrations and automated interview invitations are outside the initial MVP.

---

# 13. Dashboard and Analytics

### FR-DASH-001 — Overview Dashboard

**Priority:** P1

The dashboard should display:

- Total jobs.
- Open jobs.
- Total candidates.
- Active applications.
- Scheduled interviews.
- Hiring outcomes.

Acceptance criteria:

- Metrics are calculated from actual records.
- Results are restricted to the authorized organization.
- Empty states are handled.
- Dashboard values are not fabricated.

### FR-DASH-002 — Recruitment Funnel

**Priority:** P1

The dashboard should display application distribution across recruitment stages.

### FR-DASH-003 — Job-Level Statistics

**Priority:** P2

The system may provide job-level recruitment statistics.

### FR-DASH-004 — Data Accuracy

**Priority:** P0

All displayed metrics must be calculated from actual stored records.

Acceptance criteria:

- Counts reflect persisted records.
- Organization boundaries are enforced.
- Filtering rules are documented.
- Inconsistent or unavailable data is handled clearly.

---

# 14. Search and Filtering

### FR-SEARCH-001 — Global Candidate Search

**Priority:** P1

Authorized users should be able to search candidates within their organization.

Possible searchable fields:

- Candidate name.
- Email.
- Skills.
- Location.

### FR-SEARCH-002 — Job Search

**Priority:** P1

Authorized users should be able to search jobs.

### FR-SEARCH-003 — Application Filtering

**Priority:** P0

Users should be able to filter applications by job and recruitment stage.

### FR-SEARCH-004 — Pagination

**Priority:** P0

List endpoints must support pagination to prevent unnecessarily large responses.

Acceptance criteria:

- Pagination parameters are validated.
- Results have consistent ordering.
- Organization-level access restrictions are preserved.
- Excessively large page sizes are restricted by configuration.

---

# 15. Notifications

### FR-NOTIFY-001 — In-App Feedback

**Priority:** P1

The application should display relevant success and error feedback.

Examples:

- Successful job creation.
- Successful candidate creation.
- Resume upload completion.
- Processing failure.
- Successful application creation.
- Successful stage update.

### FR-NOTIFY-002 — Processing Status

**Priority:** P0

Users must be able to identify the status of resume processing.

Acceptance criteria:

- Processing states are visible.
- Failed operations are communicated.
- Users can distinguish completed and pending operations.

### FR-NOTIFY-003 — External Notifications

**Priority:** P2

Email and messaging notifications may be introduced in future releases.

They must not be mandatory for the zero-budget MVP.

The initial implementation does not require paid email, SMS or WhatsApp services.

---

# 16. Audit and Activity Tracking

### FR-AUDIT-001 — Important Activity Records

**Priority:** P1

The system should record important recruitment and administrative actions.

Examples:

- Job creation and updates.
- Candidate creation and updates.
- Application creation.
- Application stage changes.
- Resume processing.
- AI evaluations.
- Interview feedback.
- Relevant administrative actions.

### FR-AUDIT-002 — Actor Identification

**Priority:** P1

Important activity records should identify the responsible user where available.

### FR-AUDIT-003 — Audit Integrity

**Priority:** P1

Ordinary application users must not be able to modify historical audit records through normal application operations.

Acceptance criteria:

- Audit records are organization-scoped.
- Audit access is permission-controlled.
- Historical records are protected from ordinary modification.
- Relevant timestamps and actor information are retained.

---

# 17. Privacy and Candidate Data Operations

### FR-PRIV-001 — Data Access Restrictions

**Priority:** P0

Candidate information shall only be accessible to authorized users.

Acceptance criteria:

- Authentication is required.
- Backend authorization is enforced.
- Organization-level access restrictions are enforced.
- Resume documents follow the same access restrictions.

### FR-PRIV-002 — Data Correction

**Priority:** P1

Authorized workflows should support correction of inaccurate candidate information.

### FR-PRIV-003 — Data Deletion

**Priority:** P1

The system should support controlled deletion or anonymization according to defined retention requirements.

Acceptance criteria:

- Deletion operations are authorized.
- Related records and file references are handled consistently.
- Retention rules are respected.
- Deletion actions can be traced where appropriate.

### FR-PRIV-004 — Data Export

**Priority:** P2

The system may support candidate data export in a structured format.

### FR-PRIV-005 — Privacy Notice

**Priority:** P1

Relevant data collection workflows should provide appropriate privacy information.

These requirements are engineering controls and do not independently establish legal compliance.

Candidate-facing self-service privacy functionality is outside the initial application scope. Appropriate organizational procedures for handling candidate information must still be considered.

---

# 18. API Behaviour

All backend functionality shall be accessible through documented API contracts.

Initial API conventions:

- Base path: `/api/v1`.
- JSON request and response formats.
- Consistent HTTP status codes.
- Request validation.
- Structured error responses.
- Organization-scoped access checks.
- Pagination for list endpoints.
- Server-side authorization.
- Stable resource identifiers.

Expected success response:

```json
{
  "success": true,
  "data": {},
  "meta": {
    "request_id": "example-request-id"
  }
}
```

Expected error response:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The supplied information is invalid."
  },
  "meta": {
    "request_id": "example-request-id"
  }
}
```

Detailed routes and schemas will be defined in the API specification.

The backend must derive organization identity from authenticated context rather than trusting arbitrary client-supplied organization identifiers.

---

# 19. Error Handling

### FR-ERROR-001 — Validation Errors

Invalid requests must return structured validation errors.

### FR-ERROR-002 — Resource Not Found

Requests for unavailable or inaccessible resources must return appropriate not-found responses without exposing sensitive information.

### FR-ERROR-003 — Processing Failures

Resume and AI processing failures must be recorded and communicated clearly.

### FR-ERROR-004 — Conflict Handling

Conflicting operations, such as duplicate application creation, should return appropriate conflict responses where applicable.

### FR-ERROR-005 — Authorization Failures

Unauthorized operations must return appropriate authorization errors without revealing protected information.

---

# 20. Functional Requirement Traceability

| Requirement Group | Related Module | Verification |
|---|---|---|
| FR-AUTH | Authentication and authorization | Authentication and permission tests |
| FR-ORG | Organization management | API and permission tests |
| FR-JOB | Job management | CRUD and status tests |
| FR-CAND | Candidate management | CRUD and access tests |
| FR-RES | Resume management | Upload, storage and extraction tests |
| FR-AI | AI parsing | Schema, mock-provider and evaluation tests |
| FR-JOB-AI | Job requirement analysis | Requirement validation tests |
| FR-MATCH | Matching | Scoring and evidence tests |
| FR-APP | Applications | Application creation and access tests |
| FR-PIPE | Pipeline | State transition tests |
| FR-INT | Interviews | Interview workflow tests |
| FR-DASH | Analytics | Calculation and access tests |
| FR-SEARCH | Search | Filtering and pagination tests |
| FR-NOTIFY | Notifications | UI and processing tests |
| FR-AUDIT | Audit | Activity integrity tests |
| FR-PRIV | Privacy | Access and deletion tests |
| FR-ERROR | Error handling | API error tests |

---

# 21. Acceptance and Change Control

A functional requirement is complete only when:

1. Its expected behaviour is implemented.
2. Input validation is implemented.
3. Authorization is enforced where required.
4. Relevant data is persisted correctly.
5. Expected error cases are handled.
6. Appropriate automated tests pass.
7. The implementation is consistent with related documentation.
8. The feature respects the approved MVP scope.
9. Its documented acceptance criteria can be verified.

Changes must be reviewed for impact on:

- Database structure.
- API contracts.
- Frontend workflows.
- Authorization.
- AI behaviour.
- Existing tests.
- Product scope.
- Security and privacy.
- Related product documentation.

Any change introducing candidate-facing functionality requires an explicit product scope review.

The following documents must remain aligned:

- PRD.md
- PRODUCT_VISION.md
- TARGET_USERS_AND_PERSONAS.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- BUSINESS_RULES.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

---

# Document Governance

**Document Status:** Draft — Pending Consolidated Review

**Version:** 1.1

**End of Document**