# Niyuvia — User Workflows Specification

**Product:** Niyuvia  
**Product Category:** AI-Assisted Applicant Tracking System (ATS)  
**Document:** User Workflows Specification  
**Version:** 1.1  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Document Owner:** Product & Engineering  
**Related Documents:**
- PRD.md
- PRODUCT_VISION.md
- TARGET_USERS_AND_PERSONAS.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- BUSINESS_RULES.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

---

## 1. Purpose

This document defines the primary user journeys and operational workflows supported by Niyuvia.

It describes how authorized users interact with the platform, how information moves between system modules, and how the application should behave during successful, alternative, and failure scenarios.

These workflows provide a common reference for:

- Product design.
- Frontend implementation.
- Backend implementation.
- Database design.
- AI integration.
- Quality assurance.
- Security and privacy validation.

A workflow is considered successfully completed only when the required backend operation and its validation have succeeded. Frontend navigation or a displayed success message alone does not establish completion.

## 2. Scope and Workflow Conventions

### 2.1 MVP Scope

The initial Niyuvia MVP is an internal recruitment management application.

The following operating constraints apply:

1. Recruiters and authorized organization users are the primary application users.
2. Candidates are records managed internally by authorized recruiters.
3. Candidates do not have application login accounts.
4. Public job listings and public application submission are excluded from the initial MVP.
5. Recruiters create candidate records manually.
6. Recruiters upload candidate resumes.
7. Resume processing and AI-assisted workflows must support a mock AI provider.
8. Real paid AI API integration is optional and deferred.
9. AI-generated information requires validation and human review.
10. Recruitment decisions remain under authorized human control.

### 2.2 Workflow Structure

Each workflow defines:

- Workflow identifier.
- Primary actor.
- Objective.
- Preconditions.
- Main flow.
- Alternative and failure scenarios.
- Expected outcome.
- Related functional requirements.
- Verification approach where applicable.

### 2.3 General Processing Rules

All workflows must follow these principles:

- Backend authorization is mandatory.
- Organization boundaries must be enforced.
- Inputs must be validated on the backend.
- Invalid state transitions must be rejected.
- Failed operations must not produce false success responses.
- Important changes must be traceable.
- Duplicate records must be handled explicitly.
- AI-generated content must be validated before use.
- Sensitive candidate information must be protected.
- Retryable operations must not create duplicate records or unintended side effects.

## 3. User Roles

| Role | Primary Activities |
|---|---|
| Organization Administrator | Organization settings, user access and administrative controls |
| Recruiter | Job creation, candidate management, resume processing, matching and recruitment pipeline |
| Hiring Manager | Candidate review, interview participation and interview feedback |

### 3.1 Candidate Representation

Candidates are represented as recruitment records, not authenticated application users.

Candidate information is entered and maintained by authorized organization users.

Candidate-facing authentication, self-service profile management and public application submission are outside the initial MVP.

## 4. Workflow 01 — User Authentication

**Workflow ID:** WF-AUTH-001  
**Primary Actor:** Recruiter / Organization Administrator / Hiring Manager  
**Objective:** Allow an authorized organization user to access Niyuvia.

### Preconditions

- A registered user account exists.
- The user has valid authentication credentials.
- The user has an active organization membership.
- The account is permitted to access the application.

### Main Flow

1. User opens Niyuvia.
2. The application displays the login screen.
3. User enters authentication credentials.
4. Frontend submits the authentication request.
5. Backend or the configured authentication provider validates credentials.
6. An authenticated session is established.
7. Backend resolves the user's organization membership and role.
8. User is redirected to the authorized application dashboard.
9. Dashboard requests use the authenticated user's organization context.

### Alternative and Failure Flows

**Invalid credentials:**
- Authentication is rejected.
- A safe error message is displayed.
- No authenticated session is created.

**Inactive or unauthorized account:**
- Access is denied.
- Restricted organization information is not disclosed.

**Expired session:**
- Protected requests are rejected.
- Frontend handles the authentication failure.
- User is redirected to the login screen.

**Authentication provider unavailable:**
- The application displays an appropriate service error.
- No false authenticated state is created.

### Expected Outcome

An authorized organization user can access permitted application functionality.

### Related Requirements

FR-AUTH-001 through FR-AUTH-004.

### Verification

- Valid login test.
- Invalid credential test.
- Expired session test.
- Organization membership test.
- Role authorization test.

## 5. Workflow 02 — Organization Workspace Access

**Workflow ID:** WF-ORG-001  
**Primary Actor:** Organization Administrator  
**Objective:** Establish the organization context used by recruitment operations.

### Preconditions

- User is authenticated.
- Organization record exists.
- User has organization membership.

### Main Flow

1. User authenticates.
2. Backend retrieves the user's authorized organization membership.
3. Backend establishes the organization context.
4. Frontend loads the workspace.
5. Backend filters organization-owned records according to the authenticated context.
6. User accesses permitted workspace functionality.

### Alternative and Failure Flows

**No organization membership:**
- Access to organization-specific functionality is denied.
- User receives an appropriate access message.

**Invalid organization context:**
- Backend rejects the request.
- No organization data is returned.

**Cross-organization record access:**
- Backend denies the operation.
- The unauthorized record is not disclosed.

### Expected Outcome

The user operates only within an authorized organization context.

### Related Requirements

FR-AUTH-003 and relevant organization authorization requirements.

### Verification

- Organization membership tests.
- Cross-organization access tests.
- Tenant isolation tests.

## 6. Workflow 03 — Job Creation and Management

**Workflow ID:** WF-JOB-001  
**Primary Actor:** Recruiter  
**Objective:** Create and manage recruitment openings.

### Preconditions

- Recruiter is authenticated.
- Recruiter has job creation permission.
- Organization context is valid.

### Main Flow

1. Recruiter opens the Jobs section.
2. Recruiter selects Create Job.
3. System displays the job creation form.
4. Recruiter enters the job title.
5. Recruiter enters the job description.
6. Recruiter provides relevant department and location information.
7. Recruiter defines experience requirements.
8. Recruiter enters required and preferred skills.
9. Recruiter reviews the information.
10. Recruiter submits the form.
11. Backend validates the request.
12. Backend associates the job with the authenticated organization.
13. Backend creates the job record.
14. Frontend displays the saved job.

### Alternative and Failure Flows

**Missing required information:**
- Validation identifies missing fields.
- User is asked to complete the required information.

**Invalid data:**
- Backend rejects the request.
- Validation errors are returned.

**Unauthorized operation:**
- Backend denies job creation.

**Database failure:**
- Job creation fails safely.
- No false success message is displayed.

### Job Status Workflow

Supported initial job states:

- Draft.
- Open.
- Paused.
- Closed.

Only valid, authorized transitions are accepted.

The initial MVP does not require public job publishing or public job application functionality.

### Expected Outcome

A valid job record is created and accessible to authorized organization members.

### Related Requirements

FR-JOB-001 through FR-JOB-006.

### Verification

- Job creation tests.
- Job update tests.
- Job status transition tests.
- Organization authorization tests.
- Validation and database failure tests.

## 7. Workflow 04 — Internal Candidate Creation

**Workflow ID:** WF-CAND-001  
**Primary Actor:** Recruiter  
**Objective:** Create a candidate record within the organization's recruitment workspace.

### Preconditions

- Recruiter is authenticated.
- Recruiter has candidate management permission.
- Organization context is valid.

### Main Flow

1. Recruiter opens the Candidates section.
2. Recruiter selects Add Candidate.
3. System displays the candidate creation form.
4. Recruiter enters available candidate information.
5. Recruiter provides required information.
6. Frontend performs basic validation.
7. Recruiter submits the form.
8. Backend validates the request.
9. Backend checks for potential duplicate candidate records within the organization.
10. Backend creates the candidate record.
11. Candidate record is associated with the correct organization.
12. Frontend displays the candidate profile.

### Alternative and Failure Flows

**Potential duplicate:**
- System identifies a possible existing candidate.
- Recruiter reviews the existing record.
- Recruiter decides whether to use the existing record or continue.
- Records are never silently merged.

**Invalid email or contact information:**
- Backend rejects invalid values.
- User receives a validation message.

**Incomplete information:**
- Required information is requested.
- Optional fields may remain empty.

**Unauthorized operation:**
- Candidate creation is denied.

### Expected Outcome

A candidate record is created and accessible to authorized organization users.

### Scope Restrictions

- Candidates are not authenticated application users.
- Candidate self-registration is not supported.
- Public candidate application submission is not supported.
- Candidate login and self-service profile management are deferred.

### Related Requirements

FR-CAND-001 through FR-CAND-005.

### Verification

- Candidate creation tests.
- Candidate update tests.
- Duplicate detection tests.
- Organization isolation tests.
- Input validation tests.

## 8. Workflow 05 — Resume Upload

**Workflow ID:** WF-RES-001  
**Primary Actor:** Recruiter  
**Objective:** Upload and securely associate a candidate resume.

### Preconditions

- Recruiter is authenticated.
- Candidate record exists.
- Recruiter has resume upload permission.
- Candidate belongs to the recruiter's authorized organization.

### Main Flow

1. Recruiter opens a candidate profile.
2. Recruiter selects Upload Resume.
3. System displays the upload interface.
4. Recruiter selects a PDF or DOCX file.
5. Frontend performs basic file validation.
6. File is submitted through the configured upload mechanism.
7. Backend validates file type and size.
8. Backend verifies candidate access.
9. Backend stores the original document using the configured storage implementation.
10. Resume metadata is recorded.
11. Resume record is associated with the candidate.
12. Processing status is initialized as Pending.
13. Frontend displays the upload result.

### Alternative and Failure Flows

**Unsupported file:**
- Upload is rejected.
- User receives a clear explanation.

**File exceeds the configured size limit:**
- Upload is rejected.

**Storage failure:**
- Upload is reported as failed.
- Incomplete metadata is not treated as a successful upload.

**Unauthorized access:**
- Backend rejects the operation.

**Duplicate resume upload:**
- The system follows the defined resume versioning policy.
- Existing resume records are not silently overwritten.

### Expected Outcome

The original resume is stored securely and associated with the correct candidate.

### Related Requirements

FR-RES-001, FR-RES-002 and FR-RES-004.

### Verification

- PDF upload tests.
- DOCX upload tests.
- Invalid file tests.
- File size tests.
- Storage failure tests.
- Organization authorization tests.

## 9. Workflow 06 — Resume Text Extraction

**Workflow ID:** WF-RES-002  
**Primary Actor:** Backend Processing Service  
**Initiating User:** Recruiter  
**Objective:** Extract readable text from an uploaded resume.

### Preconditions

- Resume upload completed successfully.
- Resume metadata exists.
- Original document is accessible to the processing service.

### Main Flow

1. Processing is initiated after upload or through an authorized retry.
2. Resume status changes to Processing.
3. Backend identifies the document format.
4. Appropriate extraction library is selected.
5. Text is extracted from the document.
6. Extracted text is checked for usability.
7. Processing output is associated with the resume.
8. Processing status changes to Completed.
9. Recruiter can access the extracted information.

### Alternative and Failure Flows

**Unreadable PDF:**
- Extraction fails.
- Failure is recorded.
- Resume status changes to Failed.

**Scanned PDF:**
- Extraction may return insufficient text.
- Resume is marked for manual review or an appropriate failure state.
- OCR is deferred unless explicitly included in the MVP scope.
- Missing content is never invented.

**Empty document:**
- Processing is marked as failed or requiring manual review.

**Unexpected processing error:**
- Error is recorded.
- Original document remains preserved.

### Expected Outcome

Readable resume text is available for structured parsing, or a clear failure state is recorded.

### Related Requirements

FR-RES-003 and FR-RES-004.

### Verification

- PDF extraction tests.
- DOCX extraction tests.
- Empty document tests.
- Unreadable document tests.
- Extraction failure tests.

## 10. Workflow 07 — AI-Assisted Resume Parsing

**Workflow ID:** WF-AI-001  
**Primary Actor:** Recruiter  
**Processing Actor:** Resume Parsing Service  
**Objective:** Convert extracted resume text into structured candidate information.

### Preconditions

- Resume text extraction has completed.
- Candidate and resume records exist.
- A configured AI provider is available.
- The provider may be a mock implementation in the initial MVP.

### Main Flow

1. Recruiter opens the resume record.
2. Recruiter initiates parsing.
3. Backend retrieves extracted resume text.
4. Text is treated as untrusted input.
5. Backend prepares a structured parsing request.
6. Configured AI provider processes the request.
7. Provider returns structured information.
8. Backend validates the response against the expected Pydantic schema.
9. Invalid or unsupported fields are rejected or marked for review.
10. Parsing results are stored separately from approved candidate information.
11. Recruiter opens the parsing review screen.
12. Recruiter inspects extracted information.
13. Recruiter corrects inaccurate fields.
14. Recruiter approves the information.
15. Approved information is applied to the candidate profile.

### Expected Structured Information

- Candidate name.
- Contact information.
- Professional summary.
- Skills.
- Work experience.
- Education.
- Projects.
- Certifications.

Only information supported by the source resume should be extracted.

### Alternative and Failure Flows

**Mock provider selected:**
- Mock provider returns deterministic sample structured output.
- The same response validation process is applied.
- The workflow does not require a paid API key.

**Provider unavailable:**
- Processing is marked Failed or Retryable.
- Existing candidate information remains unchanged.

**Invalid structured output:**
- Schema validation rejects invalid output.
- Recruiter is informed that manual review is required.

**Missing fields:**
- Missing information remains unavailable.
- The system does not invent information.

**Incorrect extraction:**
- Recruiter can correct the result before approval.

**Prompt injection in resume:**
- Embedded instructions are treated as document content.
- They cannot override system instructions or authorize application operations.

### Expected Outcome

Recruiter-reviewed candidate information is stored in the candidate profile.

### Related Requirements

FR-AI-001 through FR-AI-006.

### Verification

- Mock provider integration tests.
- Schema validation tests.
- Missing field tests.
- Invalid output tests.
- Prompt injection handling tests.
- Human approval tests.
- Provider failure tests.

## 11. Workflow 08 — Job Requirement Analysis

**Workflow ID:** WF-AI-002  
**Primary Actor:** Recruiter  
**Processing Actor:** Job Analysis Service  
**Objective:** Create structured job requirements for candidate evaluation.

### Preconditions

- Job record exists.
- Job description is available.
- Recruiter has permission to manage job requirements.
- Configured analysis provider is available.

### Main Flow

1. Recruiter opens a job detail page.
2. Recruiter selects Analyze Requirements.
3. Backend retrieves the job description.
4. Job analysis service identifies relevant requirements.
5. Requirements are grouped into structured categories.
6. Backend validates the structured result.
7. Frontend displays extracted requirements.
8. Recruiter reviews the result.
9. Recruiter corrects inaccurate or missing requirements.
10. Recruiter approves the final requirement set.
11. Approved requirements are stored with a version.

### Requirement Categories

- Required skills.
- Preferred skills.
- Relevant experience.
- Responsibilities.
- Qualifications.

### Alternative and Failure Flows

**Mock provider selected:**
- Mock provider returns deterministic sample requirements.
- Recruiter reviews and approves the result.

**Incomplete job description:**
- Analysis may return incomplete information.
- Recruiter is asked to clarify requirements.

**Invalid AI output:**
- Invalid output is rejected.
- Previously approved requirements remain unchanged.

**Analysis unavailable:**
- Recruiter can enter requirements manually.

**Job updated after approval:**
- Existing approved requirements remain associated with their original version.
- Recruiter may request a new analysis and review the changes.

### Expected Outcome

The job has a reviewed and approved structured requirement set.

### Related Requirements

FR-JOB-AI-001 through FR-JOB-AI-003.

### Verification

- Mock provider tests.
- Requirement schema tests.
- Manual editing tests.
- Approval and version history tests.

## 12. Workflow 09 — Candidate-Job Matching

**Workflow ID:** WF-MATCH-001  
**Primary Actor:** Recruiter  
**Processing Actor:** Matching Service  
**Objective:** Evaluate candidate alignment with approved job requirements.

### Preconditions

- Candidate record exists.
- Job record exists.
- Both records belong to the authorized organization.
- Approved job requirements are available.
- Candidate information is available for assessment.

### Main Flow

1. Recruiter opens a job.
2. Recruiter selects a candidate for evaluation.
3. Backend retrieves approved job requirements.
4. Backend retrieves available candidate information.
5. Matching service evaluates the defined dimensions.
6. Service calculates dimension-level scores.
7. Service calculates the overall alignment score.
8. Evidence is associated with each assessment.
9. Missing information is identified separately.
10. Evaluation metadata is recorded.
11. Results are stored.
12. Recruiter views the matching assessment.
13. Recruiter reviews evidence, coverage and limitations.
14. Recruiter makes the recruitment decision.

### Initial Scoring Dimensions

| Dimension | Weight |
|---|---:|
| Required skills | 45% |
| Preferred skills | 15% |
| Relevant experience | 15% |
| Projects and responsibilities | 25% |
| **Total** | **100%** |

Weights may change only through documented and evaluated configuration changes.

### Matching Principles

- Matching must use job-related evidence.
- Protected characteristics must not be used as scoring criteria.
- Missing resume evidence must not automatically be interpreted as lack of ability.
- Alignment score must not be represented as a probability of hiring success.
- Evidence coverage must be distinguishable from the alignment score.
- Recruiters retain final decision authority.

### Alternative and Failure Flows

**Incomplete candidate information:**
- Matching proceeds only when sufficient information is available.
- Missing evidence is identified.
- Coverage limitations are displayed.

**No approved job requirements:**
- Matching cannot proceed.
- Recruiter is asked to approve requirements first.

**Matching calculation failure:**
- Failure is recorded.
- No fabricated score is displayed.

**Updated candidate or job information:**
- A new evaluation may be requested.
- Previous evaluation history is retained according to policy.

### Expected Outcome

A traceable candidate-job alignment assessment is presented to the recruiter.

The score does not automatically select, reject or hire a candidate.

### Related Requirements

FR-MATCH-001 through FR-MATCH-007.

### Verification

- Deterministic scoring tests.
- Weight validation tests.
- Evidence association tests.
- Missing information tests.
- Evaluation version tests.
- Protected characteristic exclusion tests.

## 13. Workflow 10 — Application Creation

**Workflow ID:** WF-APP-001  
**Primary Actor:** Recruiter  
**Objective:** Associate an internally managed candidate with a job opening.

### Preconditions

- Candidate exists.
- Job exists.
- Both records belong to the authorized organization.
- Recruiter has application creation permission.

### Main Flow

1. Recruiter opens a candidate profile or job detail page.
2. Recruiter selects Create Application.
3. System identifies the candidate and job.
4. Backend validates both records.
5. Backend checks duplicate application rules.
6. Backend creates the application.
7. Initial application stage is set to Applied.
8. Application is associated with the candidate and job.
9. Application creation event is recorded.
10. Frontend displays the application details.

### Alternative and Failure Flows

**Candidate not found:**
- Operation is rejected.

**Job not found or inaccessible:**
- Operation is rejected.

**Duplicate application:**
- System reports the existing application or conflict.
- Duplicate records are not silently created.

**Database failure:**
- Application creation fails safely.

### Expected Outcome

An application connects one candidate record to one job within the organization.

### Scope Restrictions

Applications are created internally by recruiters. Public application submission is not supported in the initial MVP.

### Related Requirements

FR-APP-001 through FR-APP-004.

### Verification

- Application creation tests.
- Duplicate application tests.
- Organization boundary tests.
- Initial stage validation tests.

## 14. Workflow 11 — Recruitment Pipeline Management

**Workflow ID:** WF-PIPE-001  
**Primary Actor:** Recruiter  
**Objective:** Track candidate progress through recruitment stages.

### Supported Stages

1. Applied.
2. Under Review.
3. Shortlisted.
4. Interview Scheduled.
5. Interview Completed.
6. Selected.
7. Rejected.
8. Withdrawn.
9. Hired.

### Main Flow

1. Recruiter opens the recruitment pipeline.
2. System displays authorized applications.
3. Recruiter selects an application.
4. Recruiter requests a stage change.
5. Backend checks the current stage.
6. Backend validates the requested transition.
7. Backend verifies user permissions.
8. Backend records the stage update.
9. Previous and new stages are recorded in application history.
10. Frontend updates the pipeline.

### Alternative and Failure Flows

**Invalid transition:**
- Update is rejected.
- Current application stage remains unchanged.

**Unauthorized transition:**
- Backend denies the operation.

**Concurrent update:**
- Conflicting changes are detected where supported.
- Users receive an appropriate conflict response.

**Database failure:**
- Stage remains unchanged.
- No false success response is returned.

### Expected Outcome

The application reflects the latest valid recruitment stage with traceable history.

### Related Requirements

FR-PIPE-001 through FR-PIPE-004.

### Verification

- Valid transition tests.
- Invalid transition tests.
- Permission tests.
- Concurrent update tests.
- Application history tests.

## 15. Workflow 12 — Interview Scheduling

**Workflow ID:** WF-INT-001  
**Primary Actor:** Recruiter  
**Secondary Actor:** Hiring Manager  
**Objective:** Schedule and manage a candidate interview.

### Preconditions

- Application exists.
- User has interview management permission.
- Assigned interviewer belongs to the authorized organization.

### Main Flow

1. Recruiter opens an application.
2. Recruiter selects Schedule Interview.
3. System displays the interview form.
4. Recruiter enters interview date and time.
5. Recruiter selects interview type.
6. Recruiter identifies the interviewer.
7. Recruiter enters relevant notes.
8. Recruiter submits the form.
9. Backend validates interview information.
10. Backend verifies organization and interviewer access.
11. Interview record is created.
12. Interview is associated with the application.
13. Frontend displays the scheduled interview.

### Alternative and Failure Flows

**Invalid date or time:**
- Submission is rejected.

**Unauthorized interviewer assignment:**
- Operation is rejected.

**Scheduling failure:**
- Error is displayed.
- No false confirmation is shown.

**Duplicate scheduling request:**
- Existing interview information is checked.
- Duplicate records are prevented where applicable.

### Expected Outcome

An interview record is associated with the correct application.

### Related Requirements

FR-INT-001 through FR-INT-003.

### Verification

- Interview creation tests.
- Date validation tests.
- Interviewer permission tests.
- Application association tests.

## 16. Workflow 13 — Interview Feedback

**Workflow ID:** WF-INT-002  
**Primary Actor:** Hiring Manager  
**Objective:** Record structured feedback after an interview.

### Preconditions

- Interview exists.
- User has feedback submission permission.
- Interview is accessible to the user.

### Main Flow

1. Hiring manager opens the interview record.
2. Manager reviews candidate and job information.
3. Manager opens the feedback form.
4. Manager enters interview observations.
5. Manager evaluates relevant job-related criteria.
6. Manager submits feedback.
7. Backend validates authorization and information.
8. Feedback is stored.
9. Authorized recruiters can review feedback.

### Alternative and Failure Flows

**Unauthorized user:**
- Submission is rejected.

**Missing required feedback:**
- Validation errors are displayed.

**Duplicate submission:**
- Existing feedback is updated or a new version is recorded according to the feedback policy.

**Persistence failure:**
- Feedback is not reported as successfully saved.

### Expected Outcome

Interview feedback is stored and available to authorized recruitment team members.

### Related Requirements

FR-INT-004.

### Verification

- Feedback submission tests.
- Feedback authorization tests.
- Update and version tests.
- Persistence failure tests.

## 17. Workflow 14 — Dashboard and Analytics

**Workflow ID:** WF-DASH-001  
**Primary Actor:** Recruiter  
**Objective:** View recruitment activity and performance indicators.

### Preconditions

- User is authenticated.
- User has dashboard access.
- Organization context is valid.

### Main Flow

1. Recruiter opens the dashboard.
2. Frontend requests dashboard information.
3. Backend validates organization access.
4. Backend retrieves relevant records.
5. Analytics service calculates required metrics.
6. Backend returns structured results.
7. Frontend displays dashboard statistics.
8. Recruiter reviews recruitment activity.

### Initial Dashboard Metrics

- Total jobs.
- Open jobs.
- Total candidates.
- Active applications.
- Scheduled interviews.
- Hiring outcomes.
- Application distribution by stage.

Metrics must be calculated from actual persisted records.

### Alternative and Failure Flows

**No recruitment data:**
- Empty states are displayed.
- Metrics are not fabricated.

**Calculation failure:**
- A clear error state is displayed.

**Unauthorized organization access:**
- Request is denied.

**Incomplete data:**
- Metrics are calculated using valid available records.
- Any applicable limitation is handled transparently.

### Expected Outcome

The recruiter receives an accurate summary of accessible recruitment records.

### Related Requirements

FR-DASH-001 through FR-DASH-004.

### Verification

- Metric calculation tests.
- Empty state tests.
- Organization isolation tests.
- Data consistency tests.

## 18. Workflow 15 — Candidate Information Correction

**Workflow ID:** WF-PRIV-001  
**Primary Actor:** Authorized Recruiter  
**Related Actor:** Organization Administrator  
**Objective:** Correct inaccurate candidate information.

### Preconditions

- Candidate record exists.
- User has permission to update candidate information.
- Candidate belongs to the authorized organization.

### Main Flow

1. Authorized user identifies incorrect candidate information.
2. User opens the candidate profile.
3. User selects the appropriate edit option.
4. User submits corrected information.
5. Backend validates the update.
6. Backend checks authorization.
7. Updated information is stored.
8. Relevant changes are recorded.
9. Updated candidate information is displayed.

### Alternative and Failure Flows

**Invalid information:**
- Backend rejects the update.
- Validation errors are returned.

**Unauthorized update:**
- Operation is denied.

**Persistence failure:**
- Existing candidate information remains unchanged.

### Expected Outcome

Corrected information is available to authorized users.

### Related Requirements

FR-PRIV-001 and FR-PRIV-002.

### Verification

- Candidate update tests.
- Authorization tests.
- Change history tests.
- Validation tests.

## 19. Workflow 16 — Candidate Data Deletion Request

**Workflow ID:** WF-PRIV-002  
**Primary Actor:** Authorized Organization Administrator  
**Related Actor:** Candidate / Authorized Request Handler  
**Objective:** Process a candidate data deletion request through a controlled administrative process.

### Preconditions

- Candidate record exists.
- An applicable deletion request has been received.
- Authorized personnel can review the request.

### Main Flow

1. A deletion request is received through an approved process.
2. Request details are recorded.
3. Request authenticity and authority are reviewed.
4. Applicable retention and legal requirements are considered.
5. Authorized personnel approve or reject the request.
6. Approved deletion or anonymization is performed according to the retention policy.
7. Related resume storage and derived information are considered.
8. Processing outcome is recorded.
9. Requester receives appropriate communication where applicable.

### Alternative and Failure Flows

**Request cannot be verified:**
- Processing is paused until verification.

**Retention requirement applies:**
- Deletion is deferred or limited as appropriate.
- The reason is recorded.

**Deletion failure:**
- Failure is recorded.
- The operation is not reported as completed.

**Related data remains:**
- The system identifies applicable associated records.
- Deletion is not reported as complete until the defined deletion procedure has been satisfied.

### Expected Outcome

The request is handled through a controlled and traceable process.

### Scope Restrictions

Candidate self-service deletion through a candidate account is not supported because candidate accounts are outside the initial MVP.

### Related Requirements

FR-PRIV-003.

### Verification

- Request recording tests.
- Authorization tests.
- Retention handling tests.
- Resume deletion tests.
- Derived data handling tests.
- Deletion failure tests.

## 20. Workflow 17 — AI Provider Configuration and Mock Processing

**Workflow ID:** WF-AI-003  
**Primary Actor:** Developer / System Administrator  
**Processing Actor:** AI Provider Adapter  
**Objective:** Allow AI-related MVP workflows to run without requiring paid external AI services.

### Preconditions

- Application backend is configured.
- An AI provider implementation is available.
- Provider configuration is valid.

### Main Flow

1. Application starts.
2. Backend loads the configured AI provider.
3. Provider adapter initializes the selected implementation.
4. Mock provider is selected for zero-cost development.
5. Resume parsing or job analysis requests are submitted through the provider interface.
6. Mock provider returns deterministic structured responses.
7. Backend validates returned data using the same validation rules used for other providers.
8. Workflow continues through the normal human-review process.
9. Provider execution metadata is recorded where applicable.

### Alternative and Failure Flows

**Mock provider unavailable:**
- AI operation fails safely.
- Application continues to support non-AI recruitment functionality.

**Invalid mock response:**
- Backend validation rejects the response.

**Real provider not configured:**
- Mock provider remains usable.
- No paid external API call is attempted.

**Provider implementation error:**
- Failure is recorded.
- Existing approved candidate and job information remains unchanged.

### Expected Outcome

The MVP can demonstrate resume parsing and job analysis workflows without requiring paid AI services.

### Related Requirements

FR-AI-001 through FR-AI-006 and relevant development requirements.

### Verification

- Mock provider initialization tests.
- Deterministic response tests.
- Provider interface tests.
- Invalid response tests.
- Missing API credential tests.

## 21. End-to-End Recruitment Journey

The primary Niyuvia MVP recruitment journey is:

1. Recruiter authenticates.
2. Recruiter enters the authorized organization workspace.
3. Recruiter creates a job.
4. Recruiter defines job requirements.
5. Recruiter reviews and approves structured requirements.
6. Recruiter creates a candidate record internally.
7. Recruiter uploads the candidate resume.
8. System extracts resume text.
9. AI-assisted parsing creates structured candidate information using the configured provider.
10. Recruiter reviews and approves extracted information.
11. Recruiter associates the candidate with the job by creating an application.
12. Matching service calculates candidate-job alignment.
13. Recruiter reviews matching evidence and limitations.
14. Recruiter manages application stages.
15. Recruiter schedules an interview.
16. Hiring manager submits interview feedback.
17. Recruiter records the recruitment outcome.
18. Dashboard statistics reflect actual recruitment activity.

The primary portfolio MVP must demonstrate this journey using local or otherwise free development infrastructure and a mock AI provider.

No public candidate registration or public application submission is required.

## 22. Workflow State and Data Integrity Principles

The following principles apply across all workflows:

- Backend authorization is mandatory.
- Organization boundaries must be respected.
- Tenant-owned records must be accessed only within the authorized organization context.
- Invalid state transitions must be rejected.
- Failed operations must not produce false success responses.
- Important changes must be traceable.
- Duplicate records must be handled explicitly.
- AI-generated information must be validated.
- Human review is required before AI-generated candidate information becomes approved profile information.
- Matching results must remain explainable and reviewable.
- AI scores must not independently determine hiring or rejection.
- Original resume documents must remain protected.
- Candidate information must be handled according to defined privacy and retention requirements.
- Operations that may be retried must avoid unintended duplicate side effects.
- Date and time values must use consistent timezone handling.

## 23. Workflow Verification Strategy

| Workflow | Verification Method |
|---|---|
| Authentication | Authentication integration tests |
| Organization access | Membership and tenant isolation tests |
| Job management | CRUD, validation and permission tests |
| Candidate creation | Validation, duplicate and CRUD tests |
| Resume upload | File validation and storage tests |
| Text extraction | PDF and DOCX test fixtures |
| AI parsing | Mock provider and schema validation tests |
| Job analysis | Requirement validation and approval tests |
| Candidate matching | Deterministic scoring and evidence tests |
| Application creation | Database and duplicate conflict tests |
| Pipeline management | State transition and history tests |
| Interview scheduling | Validation and authorization tests |
| Interview feedback | Permission and persistence tests |
| Dashboard | Metric calculation tests |
| Candidate correction | Authorization and update tests |
| Candidate deletion | Retention and deletion workflow tests |
| Mock AI processing | Provider initialization and deterministic output tests |

## 24. MVP Workflow Acceptance Principles

A workflow is not considered complete merely because its frontend interface exists.

Before marking a workflow as implemented:

1. Its preconditions must be enforced.
2. Its backend operation must work.
3. Inputs must be validated.
4. Authorization must be verified.
5. Organization boundaries must be respected.
6. Expected success behaviour must be tested.
7. Important failure scenarios must be tested.
8. Data persistence must be verified where applicable.
9. AI outputs must be validated and reviewed where applicable.
10. Relevant acceptance criteria and traceability records must be updated.

## 25. Deferred Workflows and Future Scope

The following workflows are not mandatory for the initial MVP:

- Candidate account registration and login.
- Public job browsing.
- Public application submission.
- Candidate self-service profile management.
- Candidate-facing application tracking.
- Automated email and WhatsApp recruitment campaigns.
- External job-board integrations.
- Paid AI provider integration.
- Advanced AI recruiter agents.
- Subscription and billing management.
- Advanced calendar integrations.
- Enterprise recruitment analytics.

These features require separate scope approval and updated requirements before implementation.

## 26. Document Governance

This document must remain consistent with:

- PRD.md
- PRODUCT_VISION.md
- TARGET_USERS_AND_PERSONAS.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- BUSINESS_RULES.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Changes to major user workflows must trigger a review of associated functional requirements, user stories, business rules, acceptance criteria and test cases.

The MVP scope document remains the authoritative reference for determining whether a feature belongs in the initial release.

### Change Control

Any workflow change must identify:

- Reason for change.
- Affected workflow identifiers.
- Affected functional requirements.
- Affected user stories.
- Data model implications.
- Security and privacy implications.
- Required regression tests.

---

**Document Status:** Draft — Pending Review  
**Version:** 1.1

**End of Document**