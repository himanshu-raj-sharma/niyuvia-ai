# Niyuvia — User Stories and Acceptance Criteria

**Product:** Niyuvia  
**Document:** User Stories and Acceptance Criteria  
**Version:** 1.1  
**Status:** Draft — Pending Consolidated Review  
**Date:** 1 November 2026  
**Related Documents:** PRD.md, FUNCTIONAL_REQUIREMENTS.md, PRODUCT_VISION.md, TARGET_USERS_AND_PERSONAS.md, USER_WORKFLOWS.md, BUSINESS_RULES.md

---

# 1. Purpose

This document translates Niyuvia's functional requirements into user-focused stories and testable acceptance criteria.

It provides a reference for:

- Product development.
- Frontend implementation.
- Backend implementation.
- Database design.
- Quality assurance.
- Automated testing.
- Product validation.

Each user story must represent a real user need and map to one or more functional requirements.

# 2. User Roles and Story Format

## 2.1 Application User Roles

The initial MVP supports three authenticated application roles:

1. Organization Administrator.
2. Recruiter.
3. Hiring Manager.

Candidates are external data subjects whose information is managed by authorized organization users.

Candidates are not authenticated application users in the initial MVP.

## 2.2 User Story Format

Each story follows this format:

**As a** [user role],

**I want** [functionality],

**So that** [expected benefit].

## 2.3 Priority Definitions

| Priority | Meaning |
|---|---|
| P0 | Critical for MVP |
| P1 | Important for MVP |
| P2 | Optional or future enhancement |

## 2.4 Acceptance Criteria

Acceptance criteria define observable and testable outcomes.

A story is not complete merely because its user interface exists. The expected backend behaviour, validation, authorization and data persistence must also work.

# 3. Authentication Stories

## US-AUTH-001 — User Login

**As a** registered organization user,

**I want** to log in securely,

**So that** I can access my organization's recruitment workspace.

**Priority:** P0

Acceptance criteria:

- Valid credentials allow login.
- Invalid credentials are rejected.
- Successful login establishes an authenticated session.
- Protected pages cannot be accessed without authentication.
- Authentication errors do not expose sensitive information.
- Candidate accounts are not supported in the initial MVP.

**Requirement:** FR-AUTH-001.

## US-AUTH-002 — User Logout

**As a** logged-in user,

**I want** to log out,

**So that** my session cannot be reused by someone accessing my device.

**Priority:** P0

Acceptance criteria:

- Logout terminates or invalidates the session.
- Protected resources require authentication again.
- The frontend updates its authentication state.
- Invalidated sessions cannot be reused.

**Requirement:** FR-AUTH-002.

## US-AUTH-003 — Role-Based Access

**As an** organization administrator,

**I want** users to have appropriate permissions,

**So that** sensitive recruitment operations are restricted.

**Priority:** P0

Acceptance criteria:

- Roles are associated with authorized users.
- Backend permission checks are enforced.
- Unauthorized operations are rejected.
- Frontend visibility does not replace backend authorization.
- Role permissions are documented.

**Requirement:** FR-AUTH-004.

## US-AUTH-004 — Organization Data Isolation

**As an** organization administrator,

**I want** my organization's information isolated from other organizations,

**So that** confidential recruitment records cannot be accessed by unauthorized organizations.

**Priority:** P0

Acceptance criteria:

- Organization-owned records are associated with an organization.
- Organization context is derived from trusted authentication context.
- Cross-organization access is rejected.
- Changing a client-supplied organization identifier cannot bypass authorization.

**Requirement:** FR-AUTH-005.

## US-AUTH-005 — Authentication State

**As a** registered organization user,

**I want** the application to maintain my authentication state correctly,

**So that** I can navigate the workspace securely.

**Priority:** P0

Acceptance criteria:

- Authenticated users can access permitted pages.
- Unauthenticated users cannot access protected pages.
- Expired sessions are handled appropriately.
- Backend authorization is always enforced.

**Requirement:** FR-AUTH-003.

# 4. Organization Management Stories

## US-ORG-001 — View Organization

**As an** organization administrator,

**I want** to view organization information,

**So that** I can verify my workspace details.

**Priority:** P1

Acceptance criteria:

- Organization information is displayed.
- Only authorized organization members can access it.
- Information belongs to the authenticated organization.

**Requirement:** FR-ORG-001.

## US-ORG-002 — Manage Organization Members

**As an** organization administrator,

**I want** to manage organization members,

**So that** the correct people can access Niyuvia.

**Priority:** P2

Acceptance criteria:

- Authorized administrators can manage membership.
- Role assignments are validated.
- Unauthorized users cannot modify membership.
- Membership changes are associated with the correct organization.

**Requirement:** FR-ORG-002.

## US-ORG-003 — Update Organization Profile

**As an** organization administrator,

**I want** to update organization information,

**So that** my organization's workspace details remain accurate.

**Priority:** P1

Acceptance criteria:

- Authorized administrators can update permitted fields.
- Invalid information is rejected.
- Changes persist after refresh.
- Unauthorized users cannot update organization details.

**Requirement:** FR-ORG-001.

# 5. Job Management Stories

## US-JOB-001 — Create Job

**As a** recruiter,

**I want** to create a job opening,

**So that** I can organize a new recruitment requirement.

**Priority:** P0

Acceptance criteria:

- Job title and required fields are validated.
- Job description can be entered.
- Required and preferred skills can be specified.
- The job is associated with the correct organization.
- A successful creation displays the saved job.
- New jobs use the appropriate initial status.
- Job creation does not automatically publish a public job listing.

**Requirement:** FR-JOB-001.

## US-JOB-002 — View Jobs

**As a** recruiter,

**I want** to view authorized job openings,

**So that** I can monitor recruitment activities.

**Priority:** P0

Acceptance criteria:

- Organization-scoped jobs are displayed.
- Job status is visible.
- Search and filtering work correctly.
- Results support pagination.
- Unauthorized job records are not exposed.

**Requirement:** FR-JOB-002.

## US-JOB-003 — Edit Job

**As a** recruiter,

**I want** to update job information,

**So that** I can correct changing recruitment requirements.

**Priority:** P0

Acceptance criteria:

- Authorized users can update permitted fields.
- Invalid updates are rejected.
- Changes persist after refresh.
- Changes to approved requirements can be traced.

**Requirement:** FR-JOB-004.

## US-JOB-004 — Manage Job Status

**As a** recruiter,

**I want** to change job status,

**So that** I can control recruitment activity for a position.

**Priority:** P0

Acceptance criteria:

- Draft, Open, Paused and Closed states are supported.
- Invalid transitions are rejected.
- Updated status is reflected in the job listing.
- Historical applications remain associated with the job.

**Requirement:** FR-JOB-005.

## US-JOB-005 — Archive Job

**As a** recruiter,

**I want** to archive completed or discontinued jobs,

**So that** inactive positions do not clutter my workspace.

**Priority:** P1

Acceptance criteria:

- Authorized users can archive permitted jobs.
- Historical applications are preserved.
- Archived jobs remain identifiable.
- Destructive operations are not performed accidentally.

**Requirement:** FR-JOB-006.

# 6. Candidate Management Stories

## US-CAND-001 — Create Candidate

**As a** recruiter,

**I want** to create a candidate profile,

**So that** I can maintain candidate information in one place.

**Priority:** P0

Acceptance criteria:

- Required candidate information is validated.
- Candidate details are stored.
- Candidate records belong to the correct organization.
- Invalid email addresses are rejected where an email is provided.
- Only authorized users can create candidate records.
- Candidate self-registration is not available.

**Requirement:** FR-CAND-001.

## US-CAND-002 — Search Candidates

**As a** recruiter,

**I want** to search candidate records,

**So that** I can quickly find relevant candidates.

**Priority:** P0

Acceptance criteria:

- Search by candidate name works.
- Search by email works.
- Results respect organization access.
- Pagination is supported.
- Empty results are handled clearly.

**Requirement:** FR-CAND-002.

## US-CAND-003 — View Candidate Profile

**As a** recruiter,

**I want** to view a complete candidate profile,

**So that** I can understand their professional background.

**Priority:** P0

Acceptance criteria:

- Candidate details are displayed.
- Skills and experience are accessible.
- Associated resumes are visible to authorized users.
- Associated applications are displayed.
- AI extraction results are distinguishable from verified candidate information.

**Requirement:** FR-CAND-003.

## US-CAND-004 — Update Candidate Information

**As a** recruiter,

**I want** to correct candidate information,

**So that** inaccurate extracted or manually entered details can be fixed.

**Priority:** P0

Acceptance criteria:

- Authorized users can edit permitted fields.
- Changes are validated.
- Updated values are persisted.
- Important changes can be traced.
- AI-generated values do not silently overwrite approved information.

**Requirement:** FR-CAND-004.

## US-CAND-005 — Identify Duplicate Candidates

**As a** recruiter,

**I want** to receive a warning about potential duplicate candidates,

**So that** I can avoid unnecessary duplicate records.

**Priority:** P1

Acceptance criteria:

- Matching email addresses can trigger a warning.
- Potential duplicates can be reviewed.
- Records are not merged automatically.
- Duplicate detection respects organization boundaries.

**Requirement:** FR-CAND-005.

# 7. Resume Management Stories

## US-RES-001 — Upload Resume

**As a** recruiter,

**I want** to upload a candidate's resume,

**So that** I can maintain their professional document.

**Priority:** P0

Acceptance criteria:

- PDF and DOCX are supported.
- Unsupported formats are rejected.
- File size restrictions are enforced.
- The resume is associated with the correct candidate.
- Upload status is communicated.
- Unauthorized users cannot upload documents to another organization's records.

**Requirement:** FR-RES-001.

## US-RES-002 — Extract Resume Information

**As a** recruiter,

**I want** Niyuvia to extract readable resume content,

**So that** I do not have to manually re-enter all candidate information.

**Priority:** P0

Acceptance criteria:

- Supported documents are processed.
- Extracted text is available to the processing workflow.
- Unreadable documents are handled gracefully.
- Original documents are preserved.
- Extraction failures are recorded.

**Requirement:** FR-RES-003.

## US-RES-003 — Monitor Processing Status

**As a** recruiter,

**I want** to see resume processing status,

**So that** I know whether information is ready for review.

**Priority:** P0

Acceptance criteria:

- Pending, Processing, Completed and Failed states are represented.
- Failure information is communicated appropriately.
- Processing status reflects persisted backend state.

**Requirement:** FR-RES-004.

## US-RES-004 — Access Resume Securely

**As an** authorized recruiter,

**I want** to access a candidate's uploaded resume securely,

**So that** confidential candidate documents remain protected.

**Priority:** P0

Acceptance criteria:

- Access requires authorization.
- Public unauthenticated access is prevented.
- Organization boundaries are enforced.
- File identifiers cannot be used to bypass access controls.

**Requirement:** FR-RES-002.

## US-RES-005 — Review Resume Versions

**As a** recruiter,

**I want** to inspect previous resume versions,

**So that** I can track changes in candidate documents over time.

**Priority:** P1

Acceptance criteria:

- Each uploaded version can be identified.
- Previous versions are preserved.
- Extraction results reference the corresponding resume version.
- Previous versions are not silently overwritten.

**Requirement:** FR-RES-005.

## US-RES-006 — Request Resume Deletion

**As an** authorized recruiter,

**I want** to delete a resume when permitted,

**So that** candidate documents can be managed according to retention requirements.

**Priority:** P1

Acceptance criteria:

- Deletion requires authorization.
- Storage references are handled consistently.
- Related extraction information is handled appropriately.
- Relevant deletion activity is traceable.
- Retention requirements are respected.

**Requirement:** FR-RES-006.

# 8. AI Resume Parsing Stories

## US-AI-001 — Parse Resume

**As a** recruiter,

**I want** structured information extracted from a resume,

**So that** I can review candidate details efficiently.

**Priority:** P0

Acceptance criteria:

- Candidate name, skills, education and experience are extracted where available.
- Missing fields are represented appropriately.
- Structured output follows the defined schema.
- Invalid output is not silently accepted.
- The system does not invent candidate information.

**Requirement:** FR-AI-001 and FR-AI-002.

## US-AI-002 — Review Extracted Information

**As a** recruiter,

**I want** to inspect and correct parsed resume information,

**So that** inaccurate AI-generated details do not become trusted candidate records.

**Priority:** P0

Acceptance criteria:

- Extracted fields are visible.
- Editable fields can be corrected.
- Changes are validated.
- Approved information is stored.
- Unapproved extraction results remain distinguishable.

**Requirement:** FR-AI-003.

## US-AI-003 — Inspect Processing Information

**As a** recruiter,

**I want** relevant AI processing information recorded,

**So that** extraction results can be traced and evaluated.

**Priority:** P1

Acceptance criteria:

- Processing status is available.
- Model and prompt versions are recorded when applicable.
- Validation outcomes are available.
- Relevant processing timestamps are retained.

**Requirement:** FR-AI-004.

## US-AI-004 — Protect Resume Processing

**As a** recruiter,

**I want** resume processing to treat document contents as untrusted input,

**So that** malicious instructions embedded in resumes cannot control the application.

**Priority:** P0

Acceptance criteria:

- Instructions embedded in resumes cannot override system instructions.
- Resume content cannot authorize application operations.
- External document text cannot trigger unauthorized tools or actions.
- Structured results are validated before acceptance.

**Requirement:** FR-AI-005.

## US-AI-005 — Use Mock AI Processing

**As a** developer,

**I want** a mock AI provider,

**So that** I can develop and test Niyuvia without paying for external AI APIs.

**Priority:** P0

Acceptance criteria:

- The mock provider works without external API credentials.
- Predictable structured results can be generated.
- Successful processing can be tested.
- Invalid output can be simulated.
- Provider failures can be simulated.
- Business logic does not depend on one specific provider implementation.

**Requirement:** FR-AI-006.

# 9. Job Requirement Analysis Stories

## US-REQ-001 — Structure Job Requirements

**As a** recruiter,

**I want** job requirements represented in a structured format,

**So that** candidate assessments use consistent criteria.

**Priority:** P0

Acceptance criteria:

- Required skills are represented.
- Preferred skills are represented.
- Experience and responsibilities are represented.
- Extracted requirements can be reviewed.
- Structured requirements are associated with the correct job.

**Requirement:** FR-JOB-AI-001.

## US-REQ-002 — Approve Job Requirements

**As a** recruiter,

**I want** to correct and approve extracted job requirements,

**So that** matching uses requirements I have verified.

**Priority:** P0

Acceptance criteria:

- Extracted requirements are editable.
- Approved requirements are stored.
- Unapproved changes do not silently replace approved criteria.
- Matching uses approved requirements.

**Requirement:** FR-JOB-AI-002.

## US-REQ-003 — Review Requirement Changes

**As a** recruiter,

**I want** changes to approved job requirements to be traceable,

**So that** I can understand why candidate evaluation results may change.

**Priority:** P1

Acceptance criteria:

- Requirement changes can be identified.
- Previous requirement versions remain identifiable.
- Evaluations reference the requirement version used.
- Re-evaluation can use the latest approved requirements.

**Requirement:** FR-JOB-AI-003.

# 10. Candidate Matching Stories

## US-MATCH-001 — Calculate Candidate Alignment

**As a** recruiter,

**I want** to calculate candidate-job alignment,

**So that** I can organize candidates according to documented job requirements.

**Priority:** P0

Acceptance criteria:

- Matching uses the approved scoring configuration.
- The scoring calculation is reproducible for the same inputs and version.
- The overall score is displayed.
- Missing evidence is handled separately.
- The score is not represented as a probability of hiring success.

**Requirement:** FR-MATCH-001.

## US-MATCH-002 — Understand Matching Evidence

**As a** recruiter,

**I want** to inspect supporting evidence for a matching result,

**So that** I can understand why a candidate appears aligned with a job.

**Priority:** P0

Acceptance criteria:

- Dimension-level scores are displayed.
- Relevant candidate evidence is shown.
- Missing information is identified.
- Unsupported claims are not presented as verified facts.

**Requirement:** FR-MATCH-002 and FR-MATCH-004.

## US-MATCH-003 — Review Missing Information

**As a** recruiter,

**I want** missing candidate information to be identified clearly,

**So that** I can verify information rather than assuming a candidate lacks a qualification.

**Priority:** P0

Acceptance criteria:

- Missing evidence is distinct from negative evidence.
- Unavailable information is identified.
- Recruiters can manually verify information.
- Missing information is handled according to the documented scoring rules.

**Requirement:** FR-MATCH-003.

## US-MATCH-004 — Retain Matching History

**As a** recruiter,

**I want** previous matching evaluations to be traceable,

**So that** I can understand changes between evaluations.

**Priority:** P1

Acceptance criteria:

- Evaluation versions are recorded.
- Scoring configuration is identifiable.
- Previous evaluation records are preserved according to retention policy.
- The associated job requirement version can be identified.

**Requirement:** FR-MATCH-005.

## US-MATCH-005 — Retain Human Decision Control

**As a** recruiter,

**I want** to make the final candidate decision myself,

**So that** AI assessments do not independently determine hiring outcomes.

**Priority:** P0

Acceptance criteria:

- AI matching does not automatically reject candidates.
- Recruiters can inspect the evidence.
- Application decisions require an authorized human action.
- AI scores are presented as decision-support information.

**Requirement:** FR-MATCH-006.

## US-MATCH-006 — Re-Evaluate Candidate

**As a** recruiter,

**I want** to request a new candidate evaluation,

**So that** I can assess updated candidate information against current approved job requirements.

**Priority:** P1

Acceptance criteria:

- Authorized users can request re-evaluation.
- The latest approved candidate information is used.
- The latest approved job requirements are used.
- Previous evaluation records remain identifiable.
- The evaluation configuration is recorded.

**Requirement:** FR-MATCH-007.

# 11. Application Management Stories

## US-APP-001 — Create Application

**As a** recruiter,

**I want** to associate a candidate with a job,

**So that** I can track their progress for that position.

**Priority:** P0

Acceptance criteria:

- Candidate and job must exist.
- Both records must belong to the authorized organization.
- Duplicate active applications are handled according to defined rules.
- A successful application is stored.
- Only authorized internal users can create applications.
- Public application submission is not required.

**Requirement:** FR-APP-001.

## US-APP-002 — View Applications

**As a** recruiter,

**I want** to view applications,

**So that** I can monitor candidates across open positions.

**Priority:** P0

Acceptance criteria:

- Authorized applications are displayed.
- Job and candidate information are accessible.
- Filtering works correctly.
- Results are organization-scoped.

**Requirement:** FR-APP-002 and FR-APP-003.

## US-APP-003 — Track Application History

**As a** recruiter,

**I want** important application changes recorded,

**So that** I can understand its recruitment history.

**Priority:** P1

Acceptance criteria:

- Relevant stage changes are recorded.
- Acting user and timestamp are available where applicable.
- Historical events remain associated with the correct application.

**Requirement:** FR-APP-004.

# 12. Recruitment Pipeline Stories

## US-PIPE-001 — Update Application Stage

**As a** recruiter,

**I want** to move applications between recruitment stages,

**So that** I can maintain an accurate hiring pipeline.

**Priority:** P0

Acceptance criteria:

- Supported stages are available.
- Permitted transitions are validated.
- Invalid transitions are rejected.
- Changes are persisted.
- The acting user is recorded where supported.

**Requirement:** FR-PIPE-001 and FR-PIPE-002.

## US-PIPE-002 — View Pipeline History

**As a** recruiter,

**I want** to inspect previous application stages,

**So that** I can understand recruitment progress.

**Priority:** P1

Acceptance criteria:

- Previous stage is recorded.
- New stage is recorded.
- Timestamp is recorded.
- Acting user is recorded where available.

**Requirement:** FR-PIPE-003.

## US-PIPE-003 — Filter Recruitment Pipeline

**As a** recruiter,

**I want** to filter applications by job and stage,

**So that** I can focus on relevant candidates.

**Priority:** P0

Acceptance criteria:

- Job filtering works.
- Stage filtering works.
- Candidate filtering works where supported.
- Organization access is respected.
- Pagination is supported.

**Requirement:** FR-PIPE-004.

# 13. Interview Management Stories

## US-INT-001 — Schedule Interview

**As a** recruiter,

**I want** to schedule an interview for an application,

**So that** the hiring team can coordinate candidate evaluation.

**Priority:** P1

Acceptance criteria:

- An application must exist.
- Interview date and time are validated.
- Interview details are stored.
- Authorized users can view the interview.
- The interview is associated with the correct application.

**Requirement:** FR-INT-001.

## US-INT-002 — Submit Interview Feedback

**As a** hiring manager,

**I want** to submit structured interview feedback,

**So that** recruiters can consider the interview assessment.

**Priority:** P1

Acceptance criteria:

- Authorized interviewers can submit feedback.
- Feedback is associated with the correct interview.
- Submitted information can be retrieved by authorized users.
- Feedback is associated with the acting interviewer.
- Feedback does not automatically change the application stage.

**Requirement:** FR-INT-004.

## US-INT-003 — Generate Interview Questions

**As a** hiring manager,

**I want** suggested interview questions based on approved job requirements,

**So that** I can prepare for an interview.

**Priority:** P2

Acceptance criteria:

- Questions relate to approved job requirements.
- Generated content is presented as suggestions.
- Interviewers can review questions before using them.
- Generated questions do not independently determine interview outcomes.

**Requirement:** FR-INT-005.

## US-INT-004 — View Scheduled Interviews

**As a** recruiter,

**I want** to view scheduled interviews,

**So that** I can monitor upcoming candidate evaluations.

**Priority:** P1

Acceptance criteria:

- Authorized interviews are displayed.
- Interview details are accessible.
- Interviews can be associated with their applications.
- Organization boundaries are enforced.

**Requirement:** FR-INT-002.

## US-INT-005 — Update Interview Details

**As a** recruiter,

**I want** to update interview details,

**So that** scheduling changes can be managed accurately.

**Priority:** P1

Acceptance criteria:

- Authorized users can update permitted details.
- Invalid updates are rejected.
- Changes are persisted.
- Relevant historical information is retained where appropriate.

**Requirement:** FR-INT-003.

# 14. Dashboard Stories

## US-DASH-001 — View Recruitment Overview

**As a** recruiter,

**I want** to view recruitment statistics,

**So that** I can understand current hiring activity.

**Priority:** P1

Acceptance criteria:

- Job totals are calculated from stored records.
- Candidate totals are calculated from stored records.
- Active application totals are accurate.
- Interview totals are accurate.
- Results are organization-scoped.

**Requirement:** FR-DASH-001.

## US-DASH-002 — View Recruitment Funnel

**As a** recruiter,

**I want** to view application distribution across recruitment stages,

**So that** I can understand recruitment progress.

**Priority:** P1

Acceptance criteria:

- Applications are grouped by stage.
- Values reflect actual database records.
- Empty stages are represented correctly.
- Results respect organization boundaries.

**Requirement:** FR-DASH-002.

## US-DASH-003 — View Job-Level Statistics

**As a** recruiter,

**I want** to view recruitment statistics for individual jobs,

**So that** I can understand the progress of each hiring requirement.

**Priority:** P2

Acceptance criteria:

- Statistics are associated with the correct job.
- Values reflect persisted records.
- Access is organization-scoped.

**Requirement:** FR-DASH-003.

## US-DASH-004 — Trust Dashboard Calculations

**As an** organization administrator,

**I want** dashboard statistics to reflect actual recruitment records,

**So that** I can make decisions using accurate information.

**Priority:** P0

Acceptance criteria:

- Counts reflect persisted records.
- Organization boundaries are enforced.
- Calculation rules are documented.
- Missing or unavailable data is handled clearly.

**Requirement:** FR-DASH-004.

# 15. Search Stories

## US-SEARCH-001 — Search Candidate Records

**As a** recruiter,

**I want** to search candidates by relevant information,

**So that** I can locate records quickly.

**Priority:** P1

Acceptance criteria:

- Search is organization-scoped.
- Supported search fields return relevant records.
- Invalid queries are handled safely.
- Results support pagination.

**Requirement:** FR-SEARCH-001.

## US-SEARCH-002 — Search Jobs

**As a** recruiter,

**I want** to search job openings,

**So that** I can locate a particular position.

**Priority:** P1

Acceptance criteria:

- Job title search works.
- Results respect access restrictions.
- Results support pagination.

**Requirement:** FR-SEARCH-002.

## US-SEARCH-003 — Filter Applications

**As a** recruiter,

**I want** to filter application records,

**So that** I can locate applications matching specific recruitment criteria.

**Priority:** P0

Acceptance criteria:

- Job filters work.
- Stage filters work.
- Filters can be combined where supported.
- Results are organization-scoped.
- Pagination works correctly.

**Requirement:** FR-SEARCH-003 and FR-SEARCH-004.

# 16. Notifications Stories

## US-NOTIFY-001 — Receive Processing Feedback

**As a** recruiter,

**I want** to receive clear resume-processing feedback,

**So that** I know whether additional action is required.

**Priority:** P0

Acceptance criteria:

- Processing success is communicated.
- Processing failure is communicated.
- No unsupported success message is shown.
- Processing status reflects backend state.

**Requirement:** FR-NOTIFY-001 and FR-NOTIFY-002.

## US-NOTIFY-002 — Receive External Notifications

**As a** recruiter,

**I want** optional email notifications,

**So that** I can stay informed about recruitment activities.

**Priority:** P2

Acceptance criteria:

- Notifications are sent only through configured integrations.
- Delivery failures are handled.
- External notification services are not required for the zero-budget MVP.

**Requirement:** FR-NOTIFY-003.

# 17. Audit Stories

## US-AUDIT-001 — Review Activity History

**As an** organization administrator,

**I want** to inspect important activity records,

**So that** I can understand significant changes within the organization.

**Priority:** P1

Acceptance criteria:

- Relevant actions are recorded.
- Acting user and timestamp are available where applicable.
- Audit records are organization-scoped.
- Ordinary users cannot modify historical audit records through normal operations.

**Requirement:** FR-AUDIT-001 and FR-AUDIT-003.

## US-AUDIT-002 — Identify Activity Actors

**As an** organization administrator,

**I want** important actions associated with their responsible users,

**So that** I can understand who performed significant operations.

**Priority:** P1

Acceptance criteria:

- Relevant acting users are identified.
- Timestamps are recorded.
- Audit records reference the correct organization.
- Unavailable actor information is represented appropriately.

**Requirement:** FR-AUDIT-002.

# 18. Privacy Stories

Candidates do not have application accounts in the initial MVP. Privacy requests must therefore be handled through authorized organizational workflows rather than candidate self-service pages.

## US-PRIV-001 — Restrict Candidate Information

**As an** organization administrator,

**I want** candidate information accessible only to authorized users,

**So that** personal information is not exposed unnecessarily.

**Priority:** P0

Acceptance criteria:

- Candidate information requires appropriate authorization.
- Organization boundaries are enforced.
- Unauthorized access is rejected.
- Resume documents follow the same access restrictions.
- Candidate information is not publicly accessible.

**Requirement:** FR-PRIV-001.

## US-PRIV-002 — Process Candidate Data Correction Requests

**As an** organization administrator,

**I want** to process candidate information correction requests received through appropriate channels,

**So that** inaccurate or outdated information can be corrected.

**Priority:** P1

Acceptance criteria:

- Authorized users can correct permitted candidate information.
- Corrections are validated.
- Relevant changes are recorded.
- The process does not require a candidate login account.
- Correction handling follows documented organizational procedures.

**Requirement:** FR-PRIV-002.

## US-PRIV-003 — Process Candidate Data Deletion Requests

**As an** organization administrator,

**I want** to process candidate data deletion requests,

**So that** candidate information is not retained unnecessarily.

**Priority:** P1

Acceptance criteria:

- Authorized users can record and process deletion requests.
- Retention requirements are considered.
- Related records and storage references are handled consistently.
- Relevant actions are traceable.
- The workflow does not require candidate self-service access.

**Requirement:** FR-PRIV-003.

## US-PRIV-004 — Export Candidate Information

**As an** authorized organization user,

**I want** to export candidate information where permitted,

**So that** appropriate data access requests can be fulfilled.

**Priority:** P2

Acceptance criteria:

- Export is restricted to authorized users.
- Exported information is organization-scoped.
- Sensitive information is handled appropriately.
- Export operations are traceable where required.

**Requirement:** FR-PRIV-004.

## US-PRIV-005 — Provide Privacy Information

**As an** organization administrator,

**I want** relevant privacy information available for recruitment workflows,

**So that** candidate information is handled transparently.

**Priority:** P1

Acceptance criteria:

- Relevant privacy information can be maintained.
- Applicable data collection workflows reference appropriate privacy information.
- Privacy information is not represented as proof of legal compliance by itself.
- The workflow does not require candidate application accounts.

**Requirement:** FR-PRIV-005.

# 19. MVP Story Prioritization

The implementation order should prioritize:

| Phase | Story Groups |
|---|---|
| Foundation | Authentication, authorization and organization isolation |
| Recruitment Core | Job and candidate management |
| Resume Intelligence | Resume upload, extraction and parsing |
| Matching | Job requirement analysis, candidate matching and evidence |
| Recruitment Workflow | Application management and pipeline |
| Interview Management | Interview scheduling and feedback |
| Insights | Dashboard, search and analytics |
| Quality | Privacy, audit, error handling and automated testing |

P0 stories are the initial implementation priority.

P1 stories should be included where practical.

P2 stories may be deferred without blocking the initial MVP.

# 20. Acceptance Testing Principles

A user story is considered complete only when:

1. The expected workflow is implemented.
2. Required inputs are validated.
3. Permissions are enforced.
4. Expected data is stored correctly.
5. Organization boundaries are respected.
6. Error cases are handled.
7. Acceptance criteria are verified.
8. Relevant automated tests pass.

Testing must include:

- Successful workflows.
- Unauthorized access.
- Invalid input.
- Missing information.
- Duplicate records.
- Database failures.
- AI provider failures.
- File processing failures.
- Cross-organization access attempts.
- Invalid state transitions.
- Privacy-related operations.

Acceptance testing must verify backend behaviour, not only frontend appearance.

# 21. Document Governance

This document must remain consistent with:

- PRD.md
- PRODUCT_VISION.md
- TARGET_USERS_AND_PERSONAS.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_WORKFLOWS.md
- BUSINESS_RULES.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Changes to functional requirements must trigger a review of associated user stories and acceptance criteria.

New functionality outside the approved MVP must receive product scope approval before implementation.

---

**Document Status:** Draft — Pending Consolidated Review

**Version:** 1.1

**End of Document**