# Niyuvia — MVP Scope and Release Plan

**Product:** Niyuvia  
**Document:** MVP Scope and Release Plan  
**Version:** 1.1  
**Status:** Draft — Pending Consolidated Review  
**Last Updated:** 31 October 2026  
**Related Documents:** PRD.md, PRODUCT_VISION.md, FUNCTIONAL_REQUIREMENTS.md, NON_FUNCTIONAL_REQUIREMENTS.md, BUSINESS_RULES.md

---

# 1. Purpose

This document defines the boundaries, priorities, milestones and release criteria for the initial Niyuvia Minimum Viable Product.

The objective is to build a functional, demonstrable, recruiter-facing Applicant Tracking System (ATS) using a zero-budget development approach.

The initial MVP is designed for internal recruitment operations. It will allow authorized organization users to manage jobs, candidates, resumes, applications, matching evaluations and interviews from a centralized workspace.

The initial release is intended to demonstrate engineering capability, validate recruitment workflows and establish a foundation for future product development.

It is not a declaration of commercial production readiness.

# 2. Product Direction

Niyuvia V1 is an internal recruiter-facing recruitment management platform.

The initial product will serve:

1. Organization Administrators.
2. Recruiters.
3. Hiring Managers.

Candidates will not directly access the Niyuvia application in the initial MVP.

Recruiters and authorized organization users will create candidate records, upload resumes and manage applications internally.

Public candidate-facing functionality is explicitly deferred.

# 3. MVP Objectives

The initial MVP must demonstrate that Niyuvia can:

1. Authenticate authorized recruitment users.
2. Enforce role-based and organization-scoped authorization.
3. Create and manage job openings.
4. Create and manage candidate profiles internally.
5. Upload and securely manage resumes.
6. Extract resume text.
7. Parse resume information into structured data using an optional AI provider.
8. Review and correct extracted information.
9. Define structured job requirements.
10. Calculate candidate-job alignment.
11. Explain matching results using supporting evidence.
12. Manage recruitment applications.
13. Track candidates through recruitment stages.
14. Manage basic interview records and feedback.
15. Display recruitment activity through an internal dashboard.

# 4. Target MVP Users

## 4.1 Organization Administrator

Responsible for:

- Managing organization settings.
- Managing organization users.
- Assigning user roles.
- Accessing authorized recruitment records.
- Reviewing organization-level activity.

## 4.2 Recruiter

Responsible for:

- Creating and managing jobs.
- Adding candidate profiles.
- Uploading resumes.
- Reviewing extracted candidate information.
- Managing applications.
- Reviewing matching results.
- Managing recruitment stages.
- Scheduling interviews.

## 4.3 Hiring Manager

Responsible for:

- Reviewing assigned job openings.
- Reviewing relevant candidates.
- Inspecting matching evidence.
- Participating in interviews.
- Submitting interview feedback.
- Providing hiring recommendations.

Exact permissions must follow the approved role-permission matrix.

# 5. MVP Feature Scope

## 5.1 Authentication and Authorization

**Priority:** P0

Included:

- User login.
- User logout.
- Authentication state.
- Session handling.
- Role-based access control.
- Organization-scoped authorization.
- Protected application routes.
- Backend authorization checks.

## 5.2 Job Management

**Priority:** P0

Included:

- Create job.
- View job listing.
- View job details.
- Update job.
- Manage job status.
- Define required skills.
- Define preferred skills.
- Define experience requirements.
- Define job responsibilities.
- Approve structured job requirements.

## 5.3 Internal Candidate Management

**Priority:** P0

Included:

- Manually create candidate records.
- View candidate listing.
- View candidate profiles.
- Update candidate information.
- Search candidate records.
- Filter candidate records.
- Basic duplicate detection.
- Associate candidates with recruitment applications.

Candidate profiles are created and maintained by authorized organization users.

Public candidate registration is excluded.

## 5.4 Resume Management

**Priority:** P0

Included:

- PDF upload.
- DOCX upload.
- File validation.
- Configurable file size restrictions.
- Private local development storage.
- Resume-to-candidate association.
- Resume text extraction.
- Processing status.
- Authorized resume access.

Original uploaded documents must remain unchanged during extraction and processing.

## 5.5 AI-Assisted Resume Parsing

**Priority:** P0

Included:

- Structured resume information extraction.
- Pydantic schema validation.
- Parsing result storage.
- Extraction review interface.
- Manual correction.
- Approval before updating trusted candidate information.
- AI processing status and metadata.

AI provider access must remain optional.

A mock AI provider must support development and automated tests without requiring paid API access.

## 5.6 Job Requirement Analysis

**Priority:** P0

Included:

- Structured job requirements.
- Required skills.
- Preferred skills.
- Experience requirements.
- Responsibility mapping.
- Recruiter review and approval.
- Version-aware requirement changes.

Manual requirement entry must remain available when AI services are unavailable.

## 5.7 Candidate-Job Matching

**Priority:** P0

Included:

- Deterministic scoring engine.
- Configurable scoring weights.
- Dimension-level scores.
- Evidence references.
- Missing information indicators.
- Matching evaluation history.
- Recruiter review.

Initial proposed scoring weights:

| Dimension | Weight |
|---|---:|
| Required skills | 45% |
| Preferred skills | 15% |
| Relevant experience | 15% |
| Projects and responsibilities | 25% |
| Total | 100% |

The score represents alignment with documented job requirements.

It must not automatically reject candidates or independently determine hiring outcomes.

## 5.8 Application Management

**Priority:** P0

Included:

- Create application internally.
- Associate candidate with job.
- View application details.
- View application listing.
- Prevent duplicate active applications according to defined rules.
- Maintain application history.

Applications are created by authorized organization users in the initial MVP.

Public application submission is excluded.

## 5.9 Recruitment Pipeline

**Priority:** P0

Included:

- Applied.
- Under Review.
- Shortlisted.
- Interview Scheduled.
- Interview Completed.
- Selected.
- Rejected.
- Withdrawn.
- Hired.

The system must validate permitted transitions and retain relevant stage history.

## 5.10 Interview Management

**Priority:** P1

Included:

- Create interview record.
- Assign interviewer.
- Record interview date and time.
- Update interview details.
- Submit structured interview feedback.
- View interview history.

Calendar integrations and automated external notifications are excluded.

## 5.11 Dashboard and Analytics

**Priority:** P1

Included:

- Total jobs.
- Open jobs.
- Total candidates.
- Active applications.
- Scheduled interviews.
- Application stage distribution.
- Basic recruitment outcomes.

All metrics must come from actual application records.

The dashboard must respect organization-level authorization.

# 6. Explicitly Excluded from MVP

The following capabilities are outside the initial release scope.

| Feature | Release Decision | Reason |
|---|---|---|
| Public candidate portal | Deferred | Recruiter-facing MVP first |
| Candidate self-registration | Deferred | No external candidate accounts |
| Public job listings | Deferred | Not required for internal ATS |
| Public job application submission | Deferred | Applications created internally |
| Candidate application tracking portal | Deferred | Future candidate-facing capability |
| Candidate login | Deferred | Not required for V1 |
| Payroll management | Excluded | Outside recruitment product scope |
| Employee management | Deferred | Full HRMS is not the target |
| Subscription billing | Deferred | Not required for portfolio MVP |
| Paid AI infrastructure | Excluded | Zero-budget constraint |
| Advanced AI agents | Deferred | Core workflows take priority |
| Autonomous candidate rejection | Prohibited | Hiring decisions remain human-controlled |
| Automated candidate sourcing | Deferred | Requires additional integrations and validation |
| WhatsApp integration | Deferred | External integration complexity |
| Email automation | Deferred | Core workflow first |
| Calendar integrations | Deferred | Not required for first release |
| Advanced analytics | Deferred | Requires validated recruitment data |
| Native mobile applications | Deferred | Responsive web application is sufficient |
| Microservices | Excluded | Unnecessary architectural complexity |
| Kubernetes deployment | Excluded | Not justified for initial MVP |
| Enterprise SSO | Deferred | Future commercial capability |
| Advanced billing and usage metering | Deferred | Commercial-stage capability |

# 7. Zero-Budget Development Strategy

Initial development must not require paid subscriptions or infrastructure.

## 7.1 Development Environment

- Visual Studio Code.
- Git.
- GitHub Free.
- Node.js.
- Python.
- PostgreSQL running locally.
- Docker Desktop, if available and practical.
- Open-source development libraries.

## 7.2 AI Development Strategy

The AI integration must support two operating modes.

**Mode 1 — Mock Provider**

Used for:

- Local development.
- Automated tests.
- Demonstrations without external API credentials.
- Predictable test scenarios.

**Mode 2 — External API Provider**

Used only when:

- A suitable provider offers available free-tier access.
- The developer has valid credentials.
- Provider usage terms are acceptable.
- Appropriate candidate data safeguards are in place.

No paid API usage is mandatory for MVP development.

## 7.3 Local Storage Strategy

During development:

- Resume documents may use private local storage.
- Database records remain in local PostgreSQL.
- File paths must not be publicly exposed.
- Storage access must be isolated behind an application service.
- Synthetic candidate data must be used for demonstrations.

Commercial deployment will require a separate storage architecture review.

## 7.4 Deployment Strategy

The initial MVP must be fully testable locally.

An optional public portfolio demonstration may use available free hosting tiers, subject to current limits and service terms.

A paid domain, hosting plan or managed database is not required at this stage.

# 8. Development Milestones

The following is a proposed 30-day implementation target, not a fixed deadline.

## Milestone 1 — Project Foundation

**Target:** Days 1–5

Deliverables:

- Monorepo configuration.
- Frontend application initialization.
- Backend application initialization.
- Database connection.
- Initial database migrations.
- Environment configuration.
- Authentication foundation.
- Basic automated test setup.

Completion criteria:

- Frontend starts locally.
- Backend starts locally.
- Database connection succeeds.
- Initial tests pass.

## Milestone 2 — Recruitment Core

**Target:** Days 6–10

Deliverables:

- Job management.
- Internal candidate management.
- Application management.
- Basic dashboard layout.
- Organization-scoped access checks.

Completion criteria:

- Jobs can be created and updated.
- Candidates can be created and updated internally.
- Applications can associate candidates with jobs.
- Data is persisted correctly.
- Unauthorized organization access is rejected.

## Milestone 3 — Resume Intelligence

**Target:** Days 11–16

Deliverables:

- Resume upload.
- File validation.
- Text extraction.
- Processing status.
- AI provider abstraction.
- Structured parsing.
- Parsing result review.

Completion criteria:

- PDF and DOCX uploads work.
- Extraction errors are handled.
- Structured parsing results pass schema validation.
- Recruiter approval is required before applying parsed information.
- Mock provider works without external credentials.

## Milestone 4 — Matching Engine

**Target:** Days 17–21

Deliverables:

- Job requirement structure.
- Candidate matching service.
- Deterministic scoring.
- Evidence collection.
- Missing information handling.
- Evaluation history.

Completion criteria:

- Matching calculations are reproducible.
- Scores follow the documented configuration.
- Evidence can be inspected.
- No automatic rejection occurs.

## Milestone 5 — Recruitment Workflow

**Target:** Days 22–25

Deliverables:

- Recruitment pipeline.
- Application stage transitions.
- Interview management.
- Interview feedback.
- Basic dashboard metrics.

Completion criteria:

- Valid stage transitions work.
- Invalid transitions are rejected.
- Interview records are associated correctly.
- Dashboard figures reflect stored data.

## Milestone 6 — Quality and Demonstration

**Target:** Days 26–30

Deliverables:

- Automated tests.
- Security and permission checks.
- Error handling.
- Responsive UI refinement.
- Docker configuration.
- README.
- Architecture documentation.
- Synthetic sample data.
- Portfolio demonstration.

Completion criteria:

- Core recruitment workflow passes end-to-end testing.
- No development secrets are committed.
- Setup instructions work.
- Known limitations are documented.
- Public candidate-facing functionality remains outside the MVP.

# 9. Release Definitions

## 9.1 Internal Development Release

Purpose:

- Validate modules.
- Test integration.
- Identify defects.

Access:

- Developer only.

## 9.2 Portfolio MVP Release

Purpose:

- Demonstrate working internal recruitment capabilities.
- Showcase engineering decisions.
- Support job interviews and technical discussions.

Requirements:

- Core recruiter workflows function.
- Sample data is synthetic.
- Known limitations are documented.
- Security boundaries are tested.
- Setup instructions are available.
- No public candidate portal is required.

## 9.3 Commercial Production Release

Purpose:

- Support real organizations and candidate information.

This requires a separate approval process covering:

- Security assessment.
- Privacy and legal review.
- Production infrastructure.
- Backup and recovery.
- Tenant isolation.
- Operational monitoring.
- Incident response.
- AI quality evaluation.
- Support procedures.
- Cost and pricing model.
- Any required candidate-facing functionality.

The portfolio MVP must not be marketed as commercially production-ready.

# 10. MVP Success Criteria

The initial MVP will be considered technically successful when an authorized recruiter can complete this end-to-end journey:

1. Log in as a recruiter.
2. Create a job.
3. Define job requirements.
4. Create a candidate profile internally.
5. Upload a resume.
6. Extract resume text.
7. Parse structured information using the mock or configured AI provider.
8. Review and approve candidate information.
9. Associate the candidate with the job.
10. Calculate candidate-job alignment.
11. Inspect matching evidence.
12. Update the application stage.
13. Schedule an interview.
14. Submit interview feedback.
15. View updated dashboard metrics.

The journey must be demonstrated using synthetic data and must pass the relevant automated tests.

# 11. Scope Change Control

Any proposed MVP feature must be reviewed against:

1. Does it support the internal recruitment workflow?
2. Is it necessary for the portfolio demonstration?
3. Can it be implemented without paid services?
4. Does it introduce unnecessary complexity?
5. Does it create new security or privacy risks?
6. Does it delay a more important MVP feature?
7. Does it introduce candidate-facing functionality outside the approved scope?

Features that do not meet current priorities should be deferred.

Any change to the internal ATS boundary requires an explicit scope review.

# 12. Project Constraints

The initial project is subject to the following constraints:

- Zero-budget development.
- One primary developer.
- Limited development time alongside job preparation.
- Local-first development.
- Internal recruiter-facing MVP.
- Portfolio demonstration as the immediate objective.
- Commercial launch as a later possibility.

Job preparation and applications remain higher priority than meeting the proposed development timeline.

# 13. Document Governance

This document must be reviewed whenever MVP scope or release priorities change.

Changes must be checked against:

- PRD.md
- PRODUCT_VISION.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- USER_WORKFLOWS.md
- BUSINESS_RULES.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

The MVP scope must remain the authoritative reference for determining whether a feature belongs in the initial release.

---

**Document Status:** Draft — Pending Consolidated Review

**End of Document**