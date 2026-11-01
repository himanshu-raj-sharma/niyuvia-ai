# Niyuvia — Product Requirements Document (PRD)

**Product:** Niyuvia  
**Product Category:** AI-Assisted Applicant Tracking System (ATS)  
**Working Tagline:** Intelligent Hiring. Better Decisions.  
**Initial Market:** India  
**Target Customers:** Startups and Small-to-Medium Businesses (SMBs)  
**Document Status:** Draft — Pending Consolidated Review  
**Version:** 1.1  
**Date:** 31 October 2026  
**Document Owner:** Product & Engineering

---

# 1. Executive Summary

Niyuvia is an AI-assisted recruitment management platform designed to help startups and small-to-medium businesses organize their hiring processes, manage candidates, evaluate applications and make more informed recruitment decisions.

The platform aims to reduce manual recruitment administration by bringing job management, candidate profiles, resume processing, candidate-to-job matching, recruitment pipelines and interview coordination into one workspace.

Niyuvia will use artificial intelligence to assist recruiters with information extraction, candidate-job alignment and recruitment-related insights.

AI-generated information will remain reviewable and explainable. Recruiters will retain responsibility for evaluating candidates and making hiring decisions.

The initial implementation will be an internally operated, recruiter-facing portfolio MVP developed with a zero-budget constraint.

The initial MVP will not include public candidate-facing functionality. Recruiters and authorized organization users will create candidate records, upload resumes, associate candidates with jobs and manage recruitment applications internally.

Future commercial requirements and candidate-facing capabilities will be considered separately after the initial product has been implemented and validated.

# 2. Product Vision

## 2.1 Vision Statement

To make recruitment more organized, transparent and accessible for growing businesses through practical software automation and responsible artificial intelligence.

## 2.2 Mission

Build a recruitment platform that helps businesses:

- Manage recruitment activities in one workspace.
- Reduce repetitive resume-review administration.
- Understand candidate experience and relevant skills.
- Organize recruitment stages.
- Make informed decisions using traceable evidence.
- Improve visibility into recruitment performance.

## 2.3 Product Positioning

Niyuvia is an employer-focused recruitment platform, not a job-seeker productivity application or a complete Human Resource Management System (HRMS).

Its initial focus is internal recruitment workflow management supported by AI-assisted candidate information processing.

The initial MVP is intended for organization administrators, recruiters and hiring managers.

Candidates are external data subjects whose information is managed by authorized organization users.

Niyuvia V1 will not provide candidate login accounts, public job listings, public application submission or candidate self-service functionality.

# 3. Problem Statement

Growing businesses frequently manage recruitment using disconnected tools, spreadsheets, email conversations and manually maintained candidate records.

This creates several operational challenges:

1. Candidate information becomes scattered across multiple locations.
2. Recruiters spend time manually reviewing resumes.
3. Comparing candidate experience against job requirements is inconsistent.
4. Tracking candidate progress becomes difficult.
5. Interview feedback can be delayed or lost.
6. Recruitment performance is difficult to measure consistently.
7. Small businesses may not have access to sophisticated recruitment software.

Niyuvia aims to address these problems through a unified internal recruitment workspace.

These problems are product hypotheses at this stage. Their prevalence and commercial importance must be validated through customer interviews and product testing.

# 4. Target Market and Users

## 4.1 Initial Geographic Market

India.

The initial product should accommodate common Indian recruitment workflows and terminology.

Examples include:

- Indian cities and locations.
- Remote, hybrid and office-based work arrangements.
- Indian qualifications and educational terminology.
- Compensation information expressed in INR.
- CTC and expected compensation fields where appropriate.
- Notice period and joining availability where voluntarily provided.

These fields must be handled with appropriate access controls and data minimization.

## 4.2 Target Customer Segment

Initial target segment:

- Startups.
- Small businesses.
- Growing technology companies.
- Small recruitment teams.
- Businesses with approximately 5–300 employees.

This is an initial target hypothesis, not a validated market-size estimate.

## 4.3 Primary Application User Personas

Niyuvia V1 supports three primary application user personas.

### Persona A — Recruiter

**Responsibilities:**

- Create and manage job openings.
- Create and maintain candidate records internally.
- Upload and review candidate resumes.
- Review candidate-job matching assessments.
- Manage recruitment applications.
- Track recruitment progress.
- Coordinate interviews.

**Primary needs:**

- Centralized candidate management.
- Efficient resume review.
- Clear candidate-job alignment.
- Evidence-based matching explanations.
- Simple recruitment workflows.

### Persona B — Hiring Manager

**Responsibilities:**

- Define hiring requirements.
- Review assigned job openings.
- Review relevant candidates.
- Participate in interviews.
- Submit structured interview feedback.

**Primary needs:**

- Relevant candidate information.
- Evidence supporting candidate assessments.
- Access to interview feedback.
- Visibility into recruitment progress.

### Persona C — Organization Administrator

**Responsibilities:**

- Manage organization information.
- Manage team access.
- Assign user roles.
- Configure recruitment settings.
- Review organizational recruitment activity.

**Primary needs:**

- Secure organization access.
- User and role management.
- Organization-level visibility.
- Controlled access to recruitment records.

## 4.4 Candidate — External Data Subject

Candidates are individuals whose personal and professional information is maintained within Niyuvia by authorized organization users.

Candidates are not authenticated application users in the initial MVP.

Candidate information may include:

- Contact information.
- Professional experience.
- Skills.
- Education.
- Uploaded resumes.
- Recruitment application history.

**MVP boundary:**

Candidates will not have:

- Candidate login accounts.
- Self-registration functionality.
- Public application submission.
- Candidate dashboards.
- Self-service application tracking.

Recruiters and authorized organization users will create and maintain candidate records internally.

Candidate-facing functionality may be considered in a future release.

# 5. Product Objectives

## 5.1 Primary Objectives

**OBJ-001:** Provide a centralized workspace for recruitment management.

**OBJ-002:** Enable recruiters to create and manage job openings.

**OBJ-003:** Allow recruiters to maintain structured candidate profiles internally.

**OBJ-004:** Support PDF and DOCX resume uploads.

**OBJ-005:** Extract structured resume information with AI-assisted processing.

**OBJ-006:** Provide evidence-based candidate-to-job alignment assessments.

**OBJ-007:** Support recruitment pipeline management.

**OBJ-008:** Provide basic interview management and recruitment analytics.

**OBJ-009:** Demonstrate maintainable software architecture, testing and documentation.

## 5.2 Non-Objectives

The initial MVP will not attempt to become:

- A complete HRMS.
- A payroll management system.
- An employee attendance system.
- An enterprise workforce planning platform.
- An autonomous recruitment agent.
- A complete job marketplace.
- A public candidate application platform.
- A replacement for human hiring decisions.

# 6. Product Scope

## 6.1 MVP Features

The following capabilities define the initial portfolio MVP.

| Feature | Priority | MVP |
|---|---|---|
| Organization user authentication | Must Have | Yes |
| Basic organization context | Must Have | Yes |
| Role-based access foundation | Must Have | Yes |
| Job management | Must Have | Yes |
| Internal candidate management | Must Have | Yes |
| Resume upload | Must Have | Yes |
| PDF/DOCX text extraction | Must Have | Yes |
| Structured resume parsing | Must Have | Yes |
| Candidate information review | Must Have | Yes |
| Job requirement analysis | Must Have | Yes |
| Candidate-job matching | Must Have | Yes |
| Matching evidence and explanations | Must Have | Yes |
| Internal application management | Must Have | Yes |
| Recruitment pipeline | Must Have | Yes |
| Basic interview management | Should Have | Yes, simplified |
| Dashboard analytics | Should Have | Basic |
| Advanced interview automation | Could Have | No |
| Email and WhatsApp automation | Could Have | No |
| Subscription billing | Could Have | No |
| Advanced multi-tenant administration | Could Have | No |
| Public job listings | Deferred | No |
| Candidate self-registration | Deferred | No |
| Public application submission | Deferred | No |
| Candidate self-service portal | Deferred | No |

All candidate and application creation in the MVP must be performed by authorized organization users.

## 6.2 MVP Delivery Principle

The MVP must prioritize a complete, understandable recruitment workflow over a large collection of disconnected features.

A smaller number of correctly implemented features is preferable to many incomplete features.

The initial release must remain an internal recruiter-facing ATS.

# 7. Functional Requirements

Each requirement has a unique identifier and priority.

Priority definitions:

- **Must Have:** Required for the initial MVP.
- **Should Have:** Important but can be simplified.
- **Could Have:** Optional or deferred.

## 7.1 Authentication and Access

### FR-AUTH-001 — Organization User Login

The system must allow authorized organization users to authenticate.

Initial supported application roles:

- Organization Admin.
- Recruiter.
- Hiring Manager.

Priority: Must Have.

**Acceptance criteria:**

- Valid credentials establish an authenticated session.
- Invalid credentials do not grant access.
- Unauthenticated users cannot access protected application pages.
- Authentication state is handled consistently.

### FR-AUTH-002 — Logout

Authenticated users must be able to terminate their sessions.

Priority: Must Have.

**Acceptance criteria:**

- Logout invalidates the authenticated session according to the authentication mechanism.
- Protected pages cannot be accessed using an invalidated session.
- The user is redirected to an appropriate unauthenticated page.

### FR-AUTH-003 — Role-Based Access Foundation

The application must distinguish organizational roles and their permissions.

Priority: Must Have.

Initial roles:

- Organization Admin.
- Recruiter.
- Hiring Manager.

**Acceptance criteria:**

- Users have an assigned organization role.
- Protected operations validate the user's permissions on the backend.
- Unauthorized operations are rejected.
- Role permissions are documented in an authoritative permission matrix.

The frontend must not be the sole enforcement mechanism for authorization.

## 7.2 Job Management

### FR-JOB-001 — Create Job

Authorized recruiters must be able to create job openings.

Priority: Must Have.

Initial fields:

- Job title.
- Department.
- Job description.
- Required skills.
- Preferred skills.
- Experience requirements.
- Employment type.
- Work mode.
- Job location.
- Job status.

**Acceptance criteria:**

- Required fields are validated.
- Successfully created jobs are stored.
- Authorized users can view their organization's jobs.
- Jobs are associated with the owning organization.

### FR-JOB-002 — Edit Job

Authorized users must be able to update job details.

Priority: Must Have.

**Acceptance criteria:**

- Only authorized users can edit a job.
- Changes are validated.
- Updated information is persisted.
- Relevant job requirement changes can be traced.

### FR-JOB-003 — Job Status Management

Jobs must support:

- Draft.
- Open.
- Paused.
- Closed.

Priority: Must Have.

**Acceptance criteria:**

- Only permitted status transitions are allowed.
- Status changes are persisted.
- The current status is visible to authorized users.

### FR-JOB-004 — Job Detail View

Users must be able to inspect complete job information.

Priority: Must Have.

**Acceptance criteria:**

- Job details include requirements and status.
- Associated recruitment information is available to authorized users.
- Data is organization-scoped.

## 7.3 Candidate Management

### FR-CAND-001 — Internal Candidate Creation

Recruiters must be able to create candidate records internally.

Priority: Must Have.

Initial candidate fields:

- Full name.
- Email.
- Phone number.
- Location.
- Professional summary.
- Skills.
- Experience.
- Education.

Optional sensitive information must be collected only when necessary and appropriately protected.

**Acceptance criteria:**

- Authorized recruiters can create candidate records.
- Required fields are validated.
- Candidate records are associated with the owning organization.
- Candidate information can be corrected by authorized users.
- Candidate records are not created through public self-registration.

### FR-CAND-002 — Candidate Listing

Recruiters must be able to view and search candidate records.

Priority: Must Have.

**Acceptance criteria:**

- Authorized users can view organization-owned candidates.
- Search and filtering operate on stored candidate information.
- Unauthorized cross-organization access is rejected.

### FR-CAND-003 — Candidate Profile

The system must provide a detailed candidate profile.

Priority: Must Have.

The profile should include:

- Basic candidate information.
- Professional summary.
- Skills.
- Experience.
- Education.
- Associated resumes.
- Associated applications.
- Relevant evaluation history.

### FR-CAND-004 — Candidate-to-Job Association

A candidate must be associable with one or more job applications.

Priority: Must Have.

**Acceptance criteria:**

- Authorized users can create an application associating a candidate with a job.
- Candidate and job must belong to the authorized organization.
- Duplicate active applications are handled according to documented business rules.

## 7.4 Resume Management

### FR-RES-001 — Resume Upload

Recruiters must be able to upload PDF and DOCX resumes.

Priority: Must Have.

**Acceptance criteria:**

- Supported formats are validated.
- File size is restricted by configuration.
- Unsupported file types are rejected.
- Upload failures are communicated clearly.
- Upload access is restricted to authorized users.

### FR-RES-002 — Resume Text Extraction

The backend must extract readable text from supported resumes.

Priority: Must Have.

**Acceptance criteria:**

- Supported documents can be processed.
- Extraction errors are handled.
- Empty or unreadable documents are reported.
- Extracted text is associated with the correct resume.

### FR-RES-003 — Structured Resume Parsing

The system must extract structured information such as:

- Candidate name.
- Contact information.
- Skills.
- Work experience.
- Education.
- Projects.
- Certifications.

Priority: Must Have.

**Acceptance criteria:**

- Parsing output follows a defined schema.
- Invalid structured output is rejected or flagged.
- Missing information is represented explicitly.
- Extracted data retains appropriate source references.

### FR-RES-004 — Extraction Review

Recruiters must be able to inspect and correct extracted information.

Priority: Must Have.

**Acceptance criteria:**

- Extracted information is displayed for review.
- Recruiters can correct inaccurate fields.
- Unapproved extraction results are not silently treated as verified candidate information.
- Approved information is associated with the candidate record.

### FR-RES-005 — Parsing Status

The system must communicate whether resume processing is:

- Pending.
- Processing.
- Completed.
- Failed.

Priority: Must Have.

### FR-RES-006 — Original Resume Preservation

The original uploaded document must remain associated with its candidate record, subject to retention and access policies.

Priority: Must Have.

## 7.5 AI-Assisted Candidate Matching

### FR-AI-001 — Job Requirement Analysis

The system should convert job requirements into a structured representation.

Priority: Must Have.

**Acceptance criteria:**

- Required skills are represented separately from preferred skills.
- Experience requirements are structured.
- Relevant responsibilities can be identified.
- Recruiters can review and approve extracted requirements.
- Manual requirement entry remains available.

### FR-AI-002 — Candidate Matching

The system must calculate a candidate-job alignment score using defined, configurable criteria.

Priority: Must Have.

Initial proposed scoring dimensions:

| Dimension | Initial Weight |
|---|---:|
| Required skills | 45% |
| Preferred skills | 15% |
| Relevant experience | 15% |
| Project and responsibility evidence | 25% |
| Total | 100% |

These weights are initial product defaults, not scientifically validated hiring predictors.

**Acceptance criteria:**

- Matching uses approved job requirements.
- Matching uses available reviewed candidate information.
- The scoring calculation is reproducible.
- The scoring configuration is recorded.
- Missing evidence is handled separately from negative evidence.

### FR-AI-003 — Evidence-Based Explanations

Each assessment must provide supporting evidence from available candidate information.

Priority: Must Have.

Examples:

- Skills explicitly identified in the resume.
- Relevant project descriptions.
- Work responsibilities.
- Experience information.

The system must not invent evidence that is absent from candidate information.

### FR-AI-004 — Missing Information Handling

Missing resume information must not automatically be interpreted as proof that a candidate lacks a skill or qualification.

Priority: Must Have.

The system must distinguish between:

- Evidence found.
- Evidence not found.
- Information unavailable.
- Information requiring manual verification.

### FR-AI-005 — Matching History

The system should retain evaluation details, including scoring configuration and evaluation version.

Priority: Should Have.

### FR-AI-006 — Human Review

Recruiters must retain responsibility for candidate evaluation and hiring decisions.

Priority: Must Have.

The platform must not automatically reject candidates based solely on AI-generated scores.

## 7.6 Recruitment Pipeline

### FR-PIPE-001 — Application Stages

The system must support the following recruitment stages:

1. Applied.
2. Under Review.
3. Shortlisted.
4. Interview Scheduled.
5. Interview Completed.
6. Selected.
7. Rejected.
8. Withdrawn.
9. Hired.

Priority: Must Have.

### FR-PIPE-002 — Stage Updates

Authorized recruiters must be able to update application stages.

Priority: Must Have.

**Acceptance criteria:**

- Stage changes are validated against business rules.
- Invalid transitions are rejected.
- The application status is persisted.
- Authorized users can view the current stage.

### FR-PIPE-003 — Stage History

The system should retain the history of application stage changes.

Priority: Should Have.

### FR-PIPE-004 — Candidate Filtering

Recruiters should be able to filter applications by job, stage and relevant candidate information.

Priority: Must Have.

## 7.7 Interview Management

### FR-INT-001 — Interview Creation

Recruiters must be able to create interview records for applications.

Priority: Should Have.

Initial fields:

- Application.
- Interview date and time.
- Interview type.
- Interviewer.
- Interview status.
- Notes.

### FR-INT-002 — Interview Feedback

Authorized interviewers should be able to record structured feedback.

Priority: Should Have.

### FR-INT-003 — AI Interview Question Assistance

The system may generate suggested interview questions from job requirements.

Priority: Could Have.

Generated questions must be reviewed by the interviewer.

## 7.8 Dashboard and Analytics

### FR-AN-001 — Recruitment Overview

The dashboard should display:

- Total jobs.
- Open jobs.
- Total candidates.
- Active applications.
- Interviews scheduled.
- Hiring outcomes.

Priority: Should Have.

### FR-AN-002 — Recruitment Funnel

The dashboard should show candidate distribution across recruitment stages.

Priority: Should Have.

### FR-AN-003 — Data Accuracy

Analytics must be calculated from actual application and job records.

Priority: Must Have.

The MVP must not display fabricated recruitment statistics.

## 7.9 Internal Candidate Intake

The initial MVP must support internal candidate intake through authorized organization users.

The following mechanisms are within the initial scope:

- Manual candidate creation.
- Resume upload by authorized recruiters.
- Association of candidate records with job applications.

The following mechanisms are outside the initial scope:

- Public candidate registration.
- Public job application forms.
- Candidate self-service accounts.
- Candidate-facing application tracking.
- External job marketplace functionality.

# 8. User Workflows

## 8.1 Recruiter Workflow

1. Recruiter signs in.
2. Recruiter opens the dashboard.
3. Recruiter creates a job.
4. Recruiter defines or reviews job requirements.
5. Recruiter creates a candidate record internally.
6. Recruiter uploads a candidate resume.
7. The backend extracts resume text.
8. Structured candidate information is generated.
9. Recruiter reviews extracted information.
10. Recruiter corrects and approves candidate information.
11. Recruiter associates the candidate with a job.
12. Candidate-job matching is calculated.
13. Recruiter reviews the evidence and alignment score.
14. Recruiter updates the application stage.
15. Recruiter schedules interviews.
16. Interview feedback is recorded.
17. Recruitment statistics are updated.

All steps are performed through authorized internal application users.

## 8.2 Resume Processing Workflow

1. Receive an internally uploaded file.
2. Validate file type and size.
3. Store the original document securely.
4. Extract readable text.
5. Process the text using the configured parsing mechanism.
6. Validate the structured output.
7. Store extracted information and processing metadata.
8. Display the result for recruiter review.
9. Allow corrections where required.
10. Associate approved information with the candidate record.

Resume content must be treated as untrusted input. Instructions embedded in resumes must never override system instructions or trigger unauthorized actions.

## 8.3 Candidate Matching Workflow

1. Retrieve approved job requirements.
2. Retrieve reviewed candidate information.
3. Evaluate available evidence against each matching dimension.
4. Calculate the configured alignment score.
5. Record evidence and missing information.
6. Store the evaluation version.
7. Present results to the recruiter.
8. Allow the recruiter to make the final decision.

## 8.4 Internal Application Creation Workflow

1. Recruiter opens an existing candidate record or creates a new one.
2. Recruiter selects an eligible job.
3. The system validates the candidate and job association.
4. The system checks applicable duplicate application rules.
5. Recruiter confirms application creation.
6. The application is stored with its initial stage.
7. The application becomes available in the recruitment pipeline.

No public application submission endpoint is required for the initial MVP.

# 9. Non-Functional Requirements

## 9.1 Performance

**NFR-PERF-001:** Normal dashboard and CRUD operations should respond within an initial target of 2 seconds under local development conditions, excluding external dependencies.

**NFR-PERF-002:** Resume processing must provide a visible processing status.

**NFR-PERF-003:** Long-running operations must not unnecessarily block ordinary API requests.

These are development targets, not verified performance guarantees.

## 9.2 Reliability

**NFR-REL-001:** API errors must be handled consistently.

**NFR-REL-002:** Failed resume processing must not silently corrupt candidate records.

**NFR-REL-003:** Database migrations must be version-controlled.

**NFR-REL-004:** Important state changes should be traceable.

## 9.3 Maintainability

**NFR-MAINT-001:** Frontend, backend, business logic and database access must have clear responsibilities.

**NFR-MAINT-002:** Backend functionality must be organized into understandable modules.

**NFR-MAINT-003:** Configuration must be separated from application source code.

**NFR-MAINT-004:** Important architectural decisions must be documented.

## 9.4 Scalability

**NFR-SCALE-001:** Tenant ownership must be considered in the database and application architecture.

**NFR-SCALE-002:** Database queries must be designed with appropriate indexing.

**NFR-SCALE-003:** Background processing must be extensible.

The MVP does not claim enterprise-scale performance or fully implemented production multi-tenancy.

## 9.5 Accessibility and Usability

**NFR-UX-001:** The interface must be responsive across desktop, tablet and mobile layouts.

**NFR-UX-002:** Forms must provide understandable validation messages.

**NFR-UX-003:** Loading, empty, success and error states must be represented clearly.

**NFR-UX-004:** Important actions must be understandable without specialized technical knowledge.

# 10. Security and Privacy Requirements

Candidate resumes and recruitment records contain personal information. Security and privacy must be considered from the beginning.

**NFR-SEC-001 — Authentication**

Protected application resources must require authentication.

**NFR-SEC-002 — Authorization**

Users must only access operations permitted by their roles.

**NFR-SEC-003 — Tenant Isolation**

Organization-owned records must be associated with their owning organization. Access checks must prevent cross-organization data exposure.

**NFR-SEC-004 — File Security**

Uploaded documents must be validated and stored outside publicly accessible directories.

**NFR-SEC-005 — Secret Management**

Passwords, API keys and tokens must never be committed to Git.

**NFR-SEC-006 — Input Validation**

API requests and uploaded information must be validated.

**NFR-SEC-007 — Auditability**

Important security-sensitive and recruitment state changes should be traceable.

**NFR-SEC-008 — Data Minimization**

Collect only information necessary for defined recruitment purposes.

**NFR-SEC-009 — Retention**

Define appropriate retention and deletion procedures before commercial operation.

**NFR-SEC-010 — AI Data Handling**

Candidate data must not be used for external model training by default. Any future external AI provider integration must be assessed for privacy, retention, security and contractual requirements.

Additional engineering expectations:

- Candidate documents must not be publicly accessible.
- File access must be authorized on the backend.
- Candidate information must be organization-scoped.
- Sensitive configuration must be stored in environment variables.
- Synthetic data should be used for public portfolio demonstrations.
- External AI services must not receive candidate information without an approved integration and appropriate safeguards.

This document establishes engineering requirements; it does not constitute legal advice or establish legal compliance.

# 11. Responsible AI Requirements

Niyuvia's AI functionality is intended to assist human recruiters.

It must not replace human judgment.

The following principles are mandatory:

1. No automatic candidate rejection based solely on an AI score.
2. No use of protected characteristics in candidate matching.
3. Avoid indirect proxies for protected characteristics.
4. Provide explanations and supporting evidence.
5. Distinguish missing information from negative evidence.
6. Allow human review and correction.
7. Record AI processing metadata and model or prompt versions where applicable.
8. Validate AI output before storing or displaying it as structured information.
9. Treat resumes and external documents as untrusted input.
10. Do not represent an alignment score as the probability of hiring success.
11. Do not claim that extracted information is verified merely because an AI model produced it.
12. Preserve recruiter authority over application decisions.

# 12. Technology and Architecture Direction

The initial implementation is expected to use the following stack.

| Layer | Technology |
|---|---|
| Frontend | Next.js, React, TypeScript |
| Styling | Tailwind CSS, shadcn/ui |
| Backend | Python, FastAPI |
| Database | PostgreSQL |
| ORM | SQLAlchemy 2.x |
| Migrations | Alembic |
| Validation | Pydantic |
| Resume extraction | PyMuPDF, python-docx |
| AI integration | Provider abstraction |
| Backend testing | Pytest |
| Frontend testing | Playwright |
| Version control | Git, GitHub |
| Local infrastructure | Docker where useful |

## 12.1 Architectural Approach

The initial architecture will follow a modular monolith approach.

Frontend and backend will live in a single repository but remain separate applications.

The backend will separate:

- API endpoints.
- Business services.
- Database repositories.
- Database models.
- Request and response schemas.
- External integrations.

AI integrations must remain isolated behind a provider abstraction.

Microservices are not required for the initial MVP.

The architecture should remain understandable to a single developer and straightforward for another engineer to inspect and maintain.

## 12.2 Zero-Budget Development Constraint

The initial development must be possible without paid subscriptions.

Preferred approach:

- Local PostgreSQL database.
- Free development tools.
- Locally executed backend and frontend.
- Mock AI provider for initial development.
- Optional local AI experimentation where hardware permits.
- No mandatory paid LLM API.
- No mandatory paid hosting.
- No mandatory paid domain.

External API integrations may be added later when financially feasible.

The mock provider must produce deterministic test results without requiring external credentials.

## 12.3 Monorepo Structure

The frontend and backend must remain in one Git repository.

The repository should have clear boundaries between:

- `apps/web` — Frontend application.
- `apps/api` — Backend application.
- `docs` — Product, architecture and engineering documentation.
- `infrastructure` — Local infrastructure and deployment configuration.
- `scripts` — Development and maintenance scripts.

Implementation details must be documented separately in architecture decision records where appropriate.

# 13. MVP Scope and Exclusions

## 13.1 Included

- Organization user authentication.
- Basic organization context.
- Role-based access foundation.
- Job CRUD.
- Internal candidate CRUD.
- Resume upload.
- PDF and DOCX extraction.
- Structured resume parsing.
- Recruiter review of parsed information.
- Candidate-job alignment scoring.
- Evidence-based matching explanations.
- Internal application creation and management.
- Recruitment pipeline.
- Basic interview management.
- Basic recruitment dashboard.
- Automated tests and technical documentation.

## 13.2 Excluded From Initial MVP

- Public candidate portal.
- Candidate login.
- Candidate self-registration.
- Public job listings.
- Public job application submission.
- Candidate self-service application tracking.
- Paid subscriptions and billing.
- Multi-channel WhatsApp recruitment.
- Email campaign automation.
- External job board integrations.
- Autonomous candidate sourcing.
- Automated candidate rejection.
- Advanced calendar integrations.
- Payroll and HRMS functionality.
- Enterprise reporting.
- Complex multi-agent AI systems.
- Production cloud infrastructure.
- Commercial customer onboarding.
- Native mobile applications.

These may be considered in later releases following product validation and an explicit scope review.

# 14. Success Metrics

The MVP will be evaluated using measurable software and product criteria.

## 14.1 Functional Metrics

- All Must Have requirements implemented and tested.
- Successful creation and management of jobs.
- Successful internal candidate creation and management.
- Successful application creation and candidate-job association.
- Successful upload and extraction of supported resumes.
- Validated structured parsing output.
- Correct calculation of matching scores.
- Traceable evidence for matching assessments.
- Correct recruitment pipeline transitions.
- Correct organization-level access restrictions.
- Successful execution of the core recruiter workflow.

## 14.2 Quality Metrics

- Automated tests for critical backend workflows.
- No known critical security vulnerabilities at release review.
- Consistent API error responses.
- No committed secrets.
- Reproducible local development setup.
- Complete setup documentation.
- Clear separation between frontend and backend responsibilities.
- Documented known limitations.

## 14.3 AI Evaluation Metrics

Resume parsing should be evaluated against manually reviewed sample resumes.

Possible metrics:

- Field-level extraction precision.
- Field-level extraction recall.
- Skill extraction F1 score.
- Missing-field accuracy.
- Structured output validation rate.

Candidate matching should be evaluated against human-reviewed sample assessments.

Possible metrics:

- Agreement with human assessments.
- Ranking quality.
- Evidence coverage.
- Consistency across repeated evaluations.

AI quality targets must be established after creating a representative evaluation dataset.

# 15. Risks and Assumptions

| ID | Risk or assumption | Mitigation |
|---|---|---|
| RISK-001 | AI may extract incorrect information | Recruiter review and validation |
| RISK-002 | Resume formats vary significantly | Test with diverse sample documents |
| RISK-003 | LLM API access may require payment | Mock/local provider during MVP |
| RISK-004 | Scope may exceed available development time | Prioritize Must Have requirements |
| RISK-005 | Sensitive candidate data may be exposed | Access control and secure storage |
| RISK-006 | Matching scores may be misleading | Transparent criteria and human oversight |
| RISK-007 | Production infrastructure may be expensive | Local-first development |
| RISK-008 | Market demand is not yet validated | Interview potential customers |
| RISK-009 | Incorrect role permissions may expose records | Backend authorization tests |
| RISK-010 | Candidate records may be duplicated | Documented duplicate detection rules |

## 15.1 Key Assumptions

- The first application users will primarily be recruiters and hiring managers.
- Organization administrators will manage application access.
- The initial customer focus will be Indian startups and SMBs.
- The initial MVP will be internal and recruiter-facing.
- Candidate records will be created by authorized organization users.
- A portfolio MVP can be demonstrated using synthetic recruitment data.
- Local development is sufficient for the initial milestone.
- A provider-independent AI architecture will reduce future integration changes.
- Paid AI services and infrastructure are not mandatory for initial development.

These assumptions require validation as the product develops.

# 16. Dependencies

The MVP depends on:

- A working development environment.
- Node.js and Python.
- PostgreSQL.
- Resume extraction libraries.
- A defined database schema.
- Authentication and authorization.
- Structured API contracts.
- A representative synthetic sample dataset.
- Automated testing.

Paid AI services and paid infrastructure are not mandatory dependencies.

# 17. MVP Release Acceptance Criteria

The portfolio MVP may be considered ready for demonstration when:

**REL-001:** An authorized organization user can authenticate.

**REL-002:** An authorized recruiter can create, edit and view jobs.

**REL-003:** An authorized recruiter can create and view candidate records internally.

**REL-004:** Supported resumes can be uploaded and processed.

**REL-005:** Extracted information can be reviewed and corrected.

**REL-006:** Candidate-job alignment can be calculated using documented criteria.

**REL-007:** Matching results include traceable supporting evidence.

**REL-008:** Recruiters can create and manage applications and update application stages.

**REL-009:** Basic interview records can be maintained.

**REL-010:** Dashboard metrics reflect actual stored records.

**REL-011:** Critical workflows have automated test coverage.

**REL-012:** A new developer can follow the README and run the project locally.

**REL-013:** No paid service is required to demonstrate the core MVP.

**REL-014:** Known limitations are documented.

**REL-015:** Candidate records and resumes are protected by authorization checks.

**REL-016:** The initial MVP does not expose public candidate registration or public application submission.

Passing these criteria does not establish commercial production readiness.

# 18. Future Production Considerations

The following requirements must be addressed before commercial launch:

- Verified multi-tenant isolation.
- Production-grade authentication and authorization.
- Secure object storage.
- Database backups and recovery testing.
- Monitoring and observability.
- Rate limiting and abuse protection.
- Incident response procedures.
- Data retention and deletion workflows.
- Privacy notices and appropriate legal review.
- Vendor and subprocessor assessment.
- AI quality evaluation and ongoing monitoring.
- Load and performance testing.
- Deployment and rollback procedures.
- Customer support and operational processes.
- Billing and subscription management, if commercially required.
- Candidate-facing functionality, if approved for a future release.
- Candidate privacy and data-subject request workflows.
- Security assessment before processing real customer candidate data.

Production readiness must be assessed separately through implementation and verification.

# 19. Release Strategy

## Release 0.1 — Foundation

- Repository structure.
- Frontend initialization.
- Backend initialization.
- Database connection.
- Initial documentation.
- Basic testing infrastructure.

## Release 0.2 — Recruitment Core

- Authentication.
- Role-based access foundation.
- Job management.
- Internal candidate management.
- Application management.

## Release 0.3 — Resume Intelligence

- Resume upload.
- Text extraction.
- Structured parsing.
- Candidate profile review.

## Release 0.4 — AI-Assisted Matching

- Job requirement analysis.
- Candidate matching.
- Evidence explanations.
- Evaluation history.

## Release 0.5 — Recruitment Workflow

- Pipeline management.
- Interview management.
- Basic dashboard analytics.

## Release 1.0 — Portfolio MVP

- Integrated workflows.
- Automated testing.
- Documentation.
- Local demonstration.
- Deployment evaluation.

The schedule is indicative and subject to scope and testing results.

# 20. Document Governance

This PRD is the central product requirements reference for Niyuvia.

All feature development must be traceable to approved requirements.

Changes to requirements should include:

1. Requirement identifier.
2. Description of the proposed change.
3. Reason for the change.
4. Impact on existing functionality.
5. Impact on architecture and data.
6. Updated acceptance criteria.
7. Impact on MVP scope.
8. Related documentation requiring updates.

Related documentation should be updated when requirements change.

The following documents must remain aligned with this PRD:

- PRODUCT_VISION.md
- TARGET_USERS_AND_PERSONAS.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- BUSINESS_RULES.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- PRODUCT_METRICS_AND_SUCCESS_CRITERIA.md

The MVP scope document is the authoritative reference for deciding whether a feature belongs in the initial release.

---

# Approval Status

**Current Status:** Draft — Pending Consolidated Review

**Approval required before implementation:**

- Product vision.
- Target users.
- MVP scope.
- Functional requirements.
- Responsible AI requirements.
- Security and privacy requirements.
- Release acceptance criteria.
- Architecture direction.

No application feature should be treated as approved merely because it appears in a draft document.

**End of Document**