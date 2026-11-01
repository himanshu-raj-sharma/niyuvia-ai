# Niyuvia — Business Rules Specification

**Product:** Niyuvia  
**Document:** Business Rules Specification  
**Version:** 1.1  
**Status:** Draft — Pending Cross-Document Review  
**Last Updated:** 1 November 2026  
**Owner:** Product & Engineering  
**Related Documents:**
- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- DATABASE_DESIGN.md

---

# 1. Purpose

This document defines the business rules governing Niyuvia's recruitment operations.

Business rules describe the conditions, restrictions and decision logic that must be followed independently of the technical implementation.

They serve as the authoritative reference for:

- Backend business validation.
- Database integrity constraints.
- Authorization and access control.
- Recruitment workflow implementation.
- AI processing and matching behaviour.
- Quality assurance and acceptance testing.

A user interface must never be considered sufficient evidence that a business rule has been enforced. Rules requiring enforcement must be implemented and tested at the appropriate backend or database layer.

# 2. Rule Classification

| Category | Description |
|---|---|
| Authentication | Identity and session validation |
| Authorization | Roles and permissions |
| Organization | Tenant ownership and isolation |
| Jobs | Job creation and lifecycle |
| Candidates | Candidate records and management |
| Resumes | Document upload and processing |
| AI | AI-generated information handling |
| Matching | Candidate-job evaluation |
| Applications | Application creation and management |
| Pipeline | Recruitment stage transitions |
| Interviews | Scheduling and feedback |
| Analytics | Metric consistency |
| Privacy | Candidate information protection |
| Data Integrity | Relationships and transactions |
| Auditability | Traceability of important operations |
| Error Handling | Validation, conflicts and retry safety |

# 3. General Enforcement Principles

**BR-GEN-001 — Server-Side Enforcement**

All business-critical rules must be enforced by trusted backend logic. Frontend validation is supplementary.

**BR-GEN-002 — Trusted Identity**

User identity and permissions must originate from validated authentication context.

**BR-GEN-003 — No Unauthorized Cross-Tenant Access**

An operation must not expose or modify records belonging to another organization without explicit authorization.

**BR-GEN-004 — Explicit Business Outcomes**

Operations must return a meaningful success or failure outcome. The application must not report success when the requested operation has not completed.

**BR-GEN-005 — Consistent Business Logic**

The same business operation must follow the same rules regardless of whether it is initiated through the dashboard, API or another authorized integration.

# 4. Authentication and Authorization Rules

**BR-AUTH-001 — Authentication Required**

Users must be authenticated before accessing protected recruitment functionality.

**BR-AUTH-002 — Permission Enforcement**

A user may perform only operations permitted by their assigned role and permissions.

**BR-AUTH-003 — Backend Authorization**

Every protected operation must be authorized by the backend.

**BR-AUTH-004 — Session Validity**

Expired, revoked or invalid sessions must not provide access to protected resources.

**BR-AUTH-005 — Organization Membership**

A user must have valid membership or another explicitly authorized relationship with an organization before accessing its private recruitment records.

**BR-AUTH-006 — Membership Revocation**

Removing a user's organization membership must prevent subsequent unauthorized access, including access through previously established sessions according to the configured session policy.

**BR-AUTH-007 — Least Privilege**

Users must receive only the permissions necessary for their responsibilities.

**BR-AUTH-008 — Role Permission Matrix**

The exact role names, permission assignments and administrative capabilities must be documented and approved before authorization implementation is considered complete.

**Pending Decision:** Final role and permission matrix.

# 5. Organization and Tenant Rules

**BR-ORG-001 — Organization Ownership**

Every organization-owned recruitment record must be associated with an organization.

Examples include:

- Jobs.
- Candidates.
- Resumes.
- Applications.
- Interviews.
- Matching evaluations.
- Recruitment activity records.

**BR-ORG-002 — Tenant Isolation**

Users must not access another organization's private records without explicit authorization.

**BR-ORG-003 — Trusted Organization Context**

Organization ownership must be derived from trusted authentication and authorization context rather than blindly accepting an organization identifier supplied by a client.

**BR-ORG-004 — Consistent Ownership**

Related organization-owned records must belong to the same organization unless an explicitly approved cross-organization relationship exists.

**BR-ORG-005 — Membership Changes**

Membership changes must be reflected in authorization checks for subsequent operations.

**BR-ORG-006 — Tenant-Scoped Queries**

All organization-specific reads, updates and deletions must enforce organization scope.

**BR-ORG-007 — Tenant-Scoped Uniqueness**

Business uniqueness constraints must be scoped appropriately to the organization unless a documented global uniqueness requirement exists.

# 6. Job Management Rules

**BR-JOB-001 — Required Job Information**

A job must contain all required fields defined in the functional requirements before it can be created or published.

**BR-JOB-002 — Job Ownership**

Every job must belong to exactly one organization.

**BR-JOB-003 — Supported Job Statuses**

The initial MVP supports:

- DRAFT
- OPEN
- PAUSED
- CLOSED

**BR-JOB-004 — Application Eligibility**

Only OPEN jobs may receive new applications.

**BR-JOB-005 — Closed Job History**

Closing a job must not automatically delete its historical applications, interviews or matching evaluations.

**BR-JOB-006 — Requirement Approval**

Candidate matching must use an approved version of job requirements.

**BR-JOB-007 — Requirement Versioning**

Changes to approved requirements must be traceable when they affect previous matching evaluations.

**BR-JOB-008 — Job Status Validation**

The backend must validate every requested job status change.

**BR-JOB-009 — Invalid Job Operations**

Users must not publish, pause or close jobs through unauthorized or invalid operations.

**BR-JOB-010 — Historical Evaluation Integrity**

Changing job requirements must not silently rewrite previously stored matching evaluations.

**Pending Decision:** The complete permitted job-status transition matrix and whether reopening a CLOSED job is supported.

# 7. Candidate Management Rules

**BR-CAND-001 — Candidate Ownership**

Every candidate record must belong to an organization.

**BR-CAND-002 — Candidate Validation**

Required candidate information must be validated before record creation.

**BR-CAND-003 — Potential Duplicates**

Matching normalized candidate email addresses within an organization may indicate potential duplicates.

**BR-CAND-004 — No Automatic Merging**

Duplicate detection must not automatically merge candidate records.

**BR-CAND-005 — Authorized Updates**

Only authorized users may create or modify candidate records.

**BR-CAND-006 — Data Minimization**

Only information necessary for documented recruitment purposes should be collected.

**BR-CAND-007 — Candidate and Application Separation**

A candidate profile and a job application are separate business entities.

**BR-CAND-008 — Multiple Applications**

A candidate may have multiple applications for different jobs within the same organization.

**BR-CAND-009 — Candidate Data Correction**

Authorized users must be able to correct inaccurate candidate information while preserving required historical records.

**BR-CAND-010 — Candidate Deletion**

Candidate deletion must follow the documented privacy and data lifecycle policy. Related records must not be accidentally destroyed.

**BR-CAND-011 — Candidate Access**

Candidate information must only be accessible to authorized users within the relevant organization.

# 8. Resume Management Rules

**BR-RES-001 — Supported Formats**

The initial MVP accepts PDF and DOCX resumes.

**BR-RES-002 — File Validation**

Uploaded files must pass configured file type, size and content validation.

**BR-RES-003 — Private Storage**

Resume documents must not be publicly accessible by default.

**BR-RES-004 — Candidate Association**

Every resume record must be associated with an authorized candidate record.

**BR-RES-005 — Original Document Preservation**

Resume processing must not overwrite or destroy the original uploaded document.

**BR-RES-006 — Processing Status**

Resume processing supports:

- PENDING
- PROCESSING
- COMPLETED
- FAILED

**BR-RES-007 — Extraction Failure**

Unreadable or unsupported documents must not produce fabricated candidate information.

**BR-RES-008 — Processing Traceability**

The system must retain the processing outcome and relevant processing metadata.

**BR-RES-009 — Reprocessing Integrity**

Reprocessing a resume must not silently overwrite previously reviewed candidate information.

**BR-RES-010 — Upload Security**

Resume uploads must be validated and handled using the security controls defined in the security requirements document.

**BR-RES-011 — Controlled Download**

Resume access must require authorization. Temporary download links, if used, must follow the configured security policy.

**Pending Decision:** Maximum upload size, retention periods and exact reprocessing policy.

# 9. AI Processing Rules

**BR-AI-001 — Untrusted Document Content**

Resume and job-description content must be treated as untrusted input.

Instructions embedded in documents must not override application instructions or authorize unintended operations.

**BR-AI-002 — Structured Output**

AI-generated candidate information must pass schema validation before entering the review workflow.

**BR-AI-003 — Human Review**

AI-generated candidate information must be reviewable and correctable before it updates trusted candidate profile fields.

**BR-AI-004 — No Fabricated Information**

Missing, unclear or unavailable information must remain explicitly unavailable rather than being invented.

**BR-AI-005 — Processing Traceability**

Relevant AI processing metadata must be recorded, including applicable model, provider, schema and prompt versions.

**BR-AI-006 — Provider Independence**

Business logic must not depend directly on one provider's proprietary response format.

**BR-AI-007 — Provider Failure**

External AI provider failure must not make existing recruitment records unavailable.

**BR-AI-008 — Schema Validation Failure**

Invalid AI output must not be accepted as trusted candidate information.

**BR-AI-009 — Human Decision Authority**

AI-generated information must support authorized recruitment professionals and must not independently make final hiring decisions.

**BR-AI-010 — Data Minimization for AI**

Only the candidate information necessary for the approved AI operation may be sent to an external provider.

**BR-AI-011 — No Unauthorized Training**

Candidate resumes must not intentionally be used as Niyuvia model-training data without a separately reviewed and authorized purpose.

# 10. Candidate-Job Matching Rules

**BR-MATCH-001 — Approved Inputs**

Matching must use approved job requirements and available candidate information.

**BR-MATCH-002 — Initial Proposed Scoring Weights**

| Dimension | Proposed Weight |
|---|---:|
| Required skills | 45% |
| Preferred skills | 15% |
| Relevant experience | 15% |
| Projects and responsibilities | 25% |
| Total | 100% |

These weights are proposed MVP configuration values and must be approved before being treated as final product policy.

**BR-MATCH-003 — Deterministic Scoring**

Identical candidate inputs, approved job requirements and scoring configuration versions must produce identical results.

**BR-MATCH-004 — Evidence-Based Assessment**

Matching results must be supported by identifiable candidate evidence.

**BR-MATCH-005 — Missing Evidence**

Missing information must be distinguished from evidence that a requirement is not met.

**BR-MATCH-006 — No Automatic Rejection**

An AI-generated or calculated alignment score must never automatically reject a candidate.

**BR-MATCH-007 — Human Decision Authority**

Authorized recruitment professionals retain responsibility for candidate selection and rejection decisions.

**BR-MATCH-008 — Protected Characteristics**

Protected characteristics and inappropriate proxies must not be used as matching criteria.

**BR-MATCH-009 — Evaluation History**

Previous evaluations must remain identifiable by their input versions and scoring configuration versions.

**BR-MATCH-010 — Score Interpretation**

A matching score represents alignment with documented job requirements. It is not a probability of successful employment or a measurement of personal worth.

**BR-MATCH-011 — Evidence Traceability**

Each evaluation must retain sufficient evidence to explain how its result was produced.

**BR-MATCH-012 — Re-evaluation**

A new evaluation must be distinguishable from previous evaluations and must not silently overwrite their history.

**BR-MATCH-013 — Scoring Configuration**

Changes to scoring weights or scoring logic must be versioned.

**BR-MATCH-014 — Human Override**

Authorized recruiters may make decisions that differ from the matching result. The system must not prevent a human decision solely because of a score.

**Pending Decision:** Exact scoring formula, missing-evidence normalization, score rounding and any shortlist thresholds. No automatic rejection threshold is permitted.

# 11. Application Management Rules

**BR-APP-001 — Application Relationship**

An application must associate one candidate with one job within the same organization.

**BR-APP-002 — Application Eligibility**

New applications may only be created for OPEN jobs.

**BR-APP-003 — Duplicate Active Applications**

The same candidate must not have multiple active applications for the same job under the MVP duplicate-application policy.

**BR-APP-004 — Multiple Job Applications**

A candidate may apply for different jobs within the same organization.

**BR-APP-005 — Application Independence**

Modifying an unrelated candidate record must not silently corrupt application history.

**BR-APP-006 — Initial Application Stage**

A newly created application must begin in APPLIED.

**BR-APP-007 — Historical Records**

Application history must remain traceable when recruitment stages change.

**BR-APP-008 — Application Organization Consistency**

The candidate, job and application must belong to the same organization.

**BR-APP-009 — Application Creation Authorization**

Only authorized recruiters may create applications in the initial MVP.

**BR-APP-010 — Candidate Login Exclusion**

Candidates do not have application login accounts in the initial MVP.

**BR-APP-011 — Public Submission Exclusion**

Public candidate registration and public application submission are outside the initial MVP.

**BR-APP-012 — Duplicate Policy**

The definition of an active application and the handling of previously rejected, withdrawn or hired applications must be explicit and consistent with the pipeline rules.

**Pending Decision:** Whether a previously rejected or withdrawn candidate can be re-added to the same job and under which conditions.

# 12. Recruitment Pipeline Rules

**BR-PIPE-001 — Supported Stages**

Applications support:

- APPLIED
- UNDER_REVIEW
- SHORTLISTED
- INTERVIEW_SCHEDULED
- INTERVIEW_COMPLETED
- SELECTED
- REJECTED
- WITHDRAWN
- HIRED

**BR-PIPE-002 — Proposed MVP Transition Policy**

| Current Stage | Permitted Next Stages |
|---|---|
| APPLIED | UNDER_REVIEW, WITHDRAWN |
| UNDER_REVIEW | SHORTLISTED, REJECTED, WITHDRAWN |
| SHORTLISTED | INTERVIEW_SCHEDULED, REJECTED, WITHDRAWN |
| INTERVIEW_SCHEDULED | INTERVIEW_COMPLETED, WITHDRAWN |
| INTERVIEW_COMPLETED | SELECTED, REJECTED, SHORTLISTED, WITHDRAWN |
| SELECTED | HIRED, REJECTED, WITHDRAWN |
| REJECTED | Terminal |
| WITHDRAWN | Terminal |
| HIRED | Terminal |

**BR-PIPE-003 — Backend Transition Validation**

The backend must validate every requested stage transition.

**BR-PIPE-004 — Transition History**

A successful stage change must record:

- Previous stage.
- New stage.
- Timestamp.
- Acting user.
- Optional reason or note.

**BR-PIPE-005 — Terminal Stages**

REJECTED, WITHDRAWN and HIRED are terminal stages for the initial MVP.

**BR-PIPE-006 — Terminal Stage Protection**

Terminal applications must not be reopened through ordinary stage-update operations.

**BR-PIPE-007 — Invalid Transitions**

An invalid transition must be rejected without partially updating the application.

**BR-PIPE-008 — Concurrent Updates**

Conflicting stage updates must be detected and handled explicitly.

**BR-PIPE-009 — Human Decision**

A stage change must be attributable to an authorized user or an explicitly approved system operation.

**Pending Decision:** Whether additional transitions, such as returning from INTERVIEW_SCHEDULED to SHORTLISTED, will be supported.

# 13. Interview Management Rules

**BR-INT-001 — Interview Association**

Every interview must be associated with a valid application.

**BR-INT-002 — Interview Authorization**

Only authorized users may create, update or access interview records.

**BR-INT-003 — Interview Feedback**

Feedback must be associated with the correct interview and authorized interviewer.

**BR-INT-004 — Feedback Integrity**

Submitted feedback must not be silently overwritten without following the configured update or versioning policy.

**BR-INT-005 — Human Assessment**

Interview feedback represents the interviewer's assessment, not an automatically generated hiring decision.

**BR-INT-006 — Application Consistency**

An interview must belong to the same organization as its associated application.

**BR-INT-007 — Interview History**

Changes to interview scheduling and status must be traceable.

**BR-INT-008 — Interview Status**

The supported interview statuses and their permitted transitions must be documented before implementation.

**Pending Decision:** Interview status model, timezone handling, cancellation and rescheduling policy.

# 14. Dashboard and Analytics Rules

**BR-DASH-001 — Database-Backed Metrics**

Dashboard metrics must be calculated from actual stored records.

**BR-DASH-002 — Organization Scope**

Analytics must include only records accessible to the authenticated organization context.

**BR-DASH-003 — Consistent Definitions**

Every displayed metric must have a documented definition and calculation method.

**BR-DASH-004 — Empty Data**

When no records exist, the dashboard must display an appropriate empty state rather than fabricated statistics.

**BR-DASH-005 — Historical Accuracy**

Historical metrics must not be silently changed by unrelated updates.

**BR-DASH-006 — Time Period Consistency**

Date-range calculations must use clearly defined timezone and boundary rules.

**BR-DASH-007 — No Misleading Metrics**

The dashboard must not present estimates or incomplete calculations as verified business outcomes.

**Pending Decision:** Final metric definitions, including time-to-hire, shortlist conversion and interview-to-offer conversion.

# 15. Privacy and Data Lifecycle Rules

**BR-PRIV-001 — Purpose Limitation**

Candidate information must be processed for documented and appropriate recruitment purposes.

**BR-PRIV-002 — Data Minimization**

Unnecessary personal information must not be collected.

**BR-PRIV-003 — Authorized Access**

Candidate information must only be accessible to authorized users.

**BR-PRIV-004 — Correction Requests**

Candidate information correction requests must be handled through an appropriate authorized process.

**BR-PRIV-005 — Deletion Requests**

Deletion requests must be reviewed against applicable legal, contractual and retention requirements.

**BR-PRIV-006 — Related Data**

Deletion workflows must consider:

- Original resumes.
- Extracted candidate information.
- Applications.
- Matching evaluations.
- Interview feedback.
- Stored files.
- Backups.
- Relevant audit records.

**BR-PRIV-007 — AI Provider Sharing**

Candidate information must not be sent to an AI provider without an approved processing configuration and appropriate privacy safeguards.

**BR-PRIV-008 — No Unapproved Model Training**

Candidate resumes must not intentionally be used as model-training data without a separately reviewed and authorized purpose.

**BR-PRIV-009 — Privacy Traceability**

Relevant data access, correction, export and deletion operations must be traceable according to the security and privacy requirements.

**BR-PRIV-010 — Retention Policy**

Candidate information and related records must follow an approved retention policy.

**BR-PRIV-011 — Data Export**

Candidate data export functionality, where required, must enforce authorization and protect exported information.

**BR-PRIV-012 — Backup Considerations**

Deletion workflows must document how relevant backup copies are handled under the approved retention and recovery policy.

**Pending Decision:** Retention periods, deletion timelines, legal exceptions and backup lifecycle.

# 16. Data Integrity Rules

**BR-DATA-001 — Valid Relationships**

All stored relationships must reference valid records.

**BR-DATA-002 — Transaction Integrity**

Operations involving multiple related records must use appropriate transaction boundaries.

**BR-DATA-003 — Identifier Integrity**

Application-generated identifiers must be unique and must not be trusted as authorization evidence.

**BR-DATA-004 — Timestamp Consistency**

Stored system timestamps must use UTC.

**BR-DATA-005 — Explicit Deletion Behaviour**

Deletion behaviour must be defined for dependent records. The system must not rely on accidental cascading deletion.

**BR-DATA-006 — Referential Integrity**

Foreign key relationships and other appropriate database constraints must enforce required record relationships.

**BR-DATA-007 — Consistent State**

A partially completed business operation must not leave related records in an invalid state.

**BR-DATA-008 — Concurrent Modification**

Operations must detect relevant conflicting updates and prevent silent data corruption.

# 17. Auditability Rules

**BR-AUDIT-001 — Important Operation Traceability**

Important recruitment and administrative operations must be traceable.

**BR-AUDIT-002 — Acting User**

Audit records must identify the acting user or explicitly identified system process where applicable.

**BR-AUDIT-003 — Operation Details**

Audit records should capture the operation type, relevant record reference and timestamp.

**BR-AUDIT-004 — Sensitive Information**

Audit logs must not unnecessarily store passwords, access tokens, resume contents or other sensitive information.

**BR-AUDIT-005 — Stage History**

Recruitment stage changes must preserve their history.

**BR-AUDIT-006 — AI Evaluation History**

AI evaluations must remain distinguishable by their relevant processing and configuration versions.

**BR-AUDIT-007 — Audit Integrity**

Ordinary application users must not be able to silently modify or delete audit history.

# 18. Error and Conflict Rules

**BR-ERROR-001 — No False Success**

An operation must not be reported as successful unless the required operation has completed.

**BR-ERROR-002 — Validation Before Persistence**

Invalid input must be rejected before it can corrupt trusted records.

**BR-ERROR-003 — Conflict Detection**

Conflicting updates must be handled explicitly.

**BR-ERROR-004 — Retry Safety**

Retryable operations should be designed to avoid duplicate records or repeated unintended side effects.

**BR-ERROR-005 — Safe Error Responses**

Error responses must not expose internal secrets, stack traces or sensitive data.

**BR-ERROR-006 — External Service Failure**

Failure of an external service must be handled without unnecessarily making existing stored recruitment information unavailable.

**BR-ERROR-007 — Partial Operation Handling**

Failed multi-step operations must follow a documented rollback, recovery or retry strategy.

# 19. MVP Rule Priorities

| Category | Priority |
|---|---|
| Authentication and authorization | P0 |
| Organization isolation | P0 |
| Job management | P0 |
| Candidate management | P0 |
| Resume validation and storage | P0 |
| AI validation and human review | P0 |
| Evidence-based matching | P0 |
| Application management | P0 |
| Pipeline validation | P0 |
| Privacy and data protection | P0 |
| Data integrity | P0 |
| Auditability of critical operations | P0 |
| Interview feedback integrity | P1 |
| Dashboard metric consistency | P1 |
| Advanced analytics | P2 |

Priority definitions:

- **P0:** Required for a safe and functional MVP.
- **P1:** Important MVP enhancement or required for the relevant implemented workflow.
- **P2:** Deferred enhancement.

# 20. Rule Verification

Business rules must be verified through appropriate tests, including:

- Backend validation tests.
- Database constraint tests.
- Authentication tests.
- Authorization tests.
- Tenant isolation tests.
- Job lifecycle tests.
- Candidate validation tests.
- Resume upload and processing tests.
- AI schema validation tests.
- Matching calculation tests.
- Application duplicate tests.
- Pipeline transition tests.
- Interview integrity tests.
- Privacy workflow tests.
- Auditability tests.
- Integration tests.
- End-to-end recruitment workflow tests.

Both valid and invalid scenarios must be tested.

A business rule must not be considered implemented merely because the corresponding user interface exists.

# 21. Cross-Document Governance

This document is the authoritative reference for Niyuvia's business-level operational rules.

It must remain consistent with:

- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- DATABASE_DESIGN.md

Changes to scoring, stage transitions, duplicate handling, authorization, tenant isolation or data lifecycle rules require review of affected documents and tests.

Every implemented rule should be traceable to its corresponding functional requirement and verification test.

# 22. Pending Business Decisions

The following decisions must be finalized before the corresponding functionality is considered implementation-ready:

| ID | Decision | Affected Area |
|---|---|---|
| TBD-001 | Organization role and permission matrix | Authorization |
| TBD-002 | Job status transition matrix | Jobs |
| TBD-003 | Candidate duplicate resolution policy | Candidates |
| TBD-004 | Resume upload size and retention limits | Resumes |
| TBD-005 | Matching formula and missing-evidence treatment | AI Matching |
| TBD-006 | Active application definition | Applications |
| TBD-007 | Reapplication policy | Applications |
| TBD-008 | Interview statuses and timezone policy | Interviews |
| TBD-009 | Dashboard metric definitions | Analytics |
| TBD-010 | Candidate retention and deletion policy | Privacy |
| TBD-011 | Backup deletion and recovery policy | Privacy |
| TBD-012 | Audit event retention policy | Auditability |

These decisions must be reviewed against the other product documents before implementation.

---

**Document Status:** Draft — Pending Cross-Document Review

**End of Document**
