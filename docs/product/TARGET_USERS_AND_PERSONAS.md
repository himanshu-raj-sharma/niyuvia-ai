# Niyuvia — Target Users and User Personas

**Product:** Niyuvia  
**Document:** Target Users and User Personas  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 31 October 2026  
**Related Documents:** PRD.md, PRODUCT_VISION.md

---

# 1. Purpose

This document defines the intended customer segments, user categories, personas, responsibilities, goals, problems and access expectations for Niyuvia.

It provides a reference for product design, feature prioritization, authorization, user experience and future customer validation.

The personas represent initial product hypotheses. They are not based on completed customer interviews or validated market research.

# 2. Target Market

## 2.1 Geographic Focus

Initial market: India.

The initial product should support common Indian recruitment workflows, terminology and organizational requirements.

## 2.2 Business Segment

Niyuvia initially targets:

- Technology startups.
- Small and medium-sized businesses.
- Growing companies with internal recruitment requirements.
- Small HR and talent acquisition teams.
- Organizations without dedicated enterprise recruitment infrastructure.

Initial target organization size: approximately 5–300 employees.

This range is a starting segmentation hypothesis.

## 2.3 Customer Characteristics

Potential customers may have:

- Multiple active job openings.
- A small recruitment team.
- Candidate information stored across spreadsheets and email.
- Repetitive resume screening activities.
- Limited recruitment analytics.
- A need for simple application tracking.
- Limited budgets for enterprise recruitment software.

# 3. User Categories

Niyuvia defines four primary user categories.

| User | Primary Responsibility |
|---|---|
| Organization Administrator | Manage organization and user access |
| Recruiter | Manage jobs, candidates and applications |
| Hiring Manager | Review candidates and provide interview feedback |
| Candidate | Submit applications and participate in recruitment |

The initial MVP primarily serves recruiters and hiring managers.

Candidate-facing functionality will remain limited during the first release.

# 4. User Personas

## 4.1 Persona A — Recruiter

**Persona ID:** USER-001  
**Role:** Recruiter / Talent Acquisition Specialist  
**User Type:** Primary  
**Priority:** Critical

### Background

Works within a startup or SMB and manages multiple job openings and candidate applications.

May handle recruitment independently or as part of a small HR team.

### Responsibilities

- Create job openings.
- Define job requirements.
- Receive candidate applications.
- Upload and review resumes.
- Maintain candidate records.
- Review candidate-job alignment.
- Manage recruitment stages.
- Coordinate interviews.
- Track hiring progress.

### Goals

- Reduce repetitive administrative work.
- Find relevant candidate information quickly.
- Organize applications.
- Compare candidates against job requirements.
- Maintain recruitment progress visibility.
- Improve coordination with hiring managers.

### Pain Points

- Candidate information scattered across tools.
- Manual resume review.
- Difficulty comparing applicants consistently.
- Repeated data entry.
- Unclear application status.
- Missing interview feedback.

### Niyuvia Value

Niyuvia provides a centralized recruitment workspace with structured candidate profiles, resume processing, evidence-based matching and pipeline management.

### Expected Features

- Recruiter dashboard.
- Job management.
- Candidate management.
- Resume upload.
- Resume parsing.
- Candidate-job matching.
- Application pipeline.
- Interview coordination.
- Recruitment analytics.

### Success Indicators

- Can create and manage a job without assistance.
- Can upload and review a candidate resume.
- Can understand matching evidence.
- Can update an application stage.
- Can identify the current status of recruitment activities.

## 4.2 Persona B — Hiring Manager

**Persona ID:** USER-002  
**Role:** Hiring Manager / Department Manager  
**User Type:** Primary  
**Priority:** High

### Background

Responsible for hiring within a department or technical team.

Usually participates in candidate evaluation and interviews but may not manage the complete recruitment workflow.

### Responsibilities

- Define role requirements.
- Review shortlisted candidates.
- Evaluate relevant experience.
- Participate in interviews.
- Submit interview feedback.
- Collaborate with recruiters.

### Goals

- Review relevant candidate information.
- Understand why a candidate was shortlisted.
- Evaluate technical or role-specific experience.
- Access interview feedback.
- Make informed hiring recommendations.

### Pain Points

- Receiving incomplete candidate information.
- Lack of structured evaluation criteria.
- Delayed interview feedback.
- Repeated discussions about candidate status.
- Limited visibility into recruitment progress.

### Niyuvia Value

Niyuvia provides structured candidate information, matching evidence and organized interview feedback.

### Expected Features

- Candidate profile viewing.
- Job requirement visibility.
- Candidate-job matching explanations.
- Interview records.
- Interview feedback submission.
- Application status visibility.

### Success Indicators

- Can review candidate information easily.
- Can understand matching explanations.
- Can provide structured interview feedback.
- Can collaborate with recruiters effectively.

## 4.3 Persona C — Organization Administrator

**Persona ID:** USER-003  
**Role:** Organization Administrator  
**User Type:** Primary  
**Priority:** High

### Background

Responsible for managing an organization's Niyuvia workspace.

May be an HR administrator, business owner or designated organization administrator.

### Responsibilities

- Manage organization settings.
- Invite authorized team members.
- Manage user roles.
- Control access permissions.
- Maintain organization-level configuration.

### Goals

- Maintain secure access.
- Manage team membership.
- Prevent unauthorized access.
- Maintain organization-level visibility.

### Pain Points

- Unclear user permissions.
- Difficulty managing team access.
- Inconsistent organization settings.
- Lack of visibility into access-related activity.

### Niyuvia Value

Niyuvia provides centralized organization and access management.

### Expected Features

- Organization profile.
- User management.
- Role management.
- Access controls.
- Organization settings.

### Success Indicators

- Can manage authorized users.
- Can assign appropriate roles.
- Can control access.
- Can view relevant organization information.

## 4.4 Persona D — Candidate

**Persona ID:** USER-004  
**Role:** Job Applicant  
**User Type:** External  
**Priority:** Medium for MVP

### Background

An individual applying for a job opening managed through Niyuvia.

Candidates are not expected to use the internal recruiter dashboard.

### Responsibilities

- Submit an application.
- Provide accurate information.
- Upload a resume.
- Participate in recruitment activities.
- Provide additional information when requested.

### Goals

- Submit applications successfully.
- Understand the application process.
- Receive appropriate recruitment communication.
- Have personal information handled responsibly.

### Pain Points

- Unclear application submission processes.
- Repeatedly providing the same information.
- Limited communication about application progress.
- Uncertainty about personal information handling.

### Niyuvia Value

Niyuvia aims to support organized candidate information handling and clear application workflows.

### Expected Features

For the initial MVP:
- Candidate information storage.
- Resume association.
- Application records.

Future possibilities:
- Public job listings.
- Candidate application portal.
- Application status visibility.
- Candidate profile management.
- Application withdrawal.

### Success Indicators

- Application information is stored correctly.
- Resume uploads are associated with the correct candidate.
- Personal information is handled appropriately.

# 5. User Goals and Product Mapping

| User | Main Goal | Niyuvia Capability |
|---|---|---|
| Recruiter | Organize recruitment | ATS and pipeline |
| Recruiter | Review resumes | Resume processing |
| Recruiter | Compare candidates | Evidence-based matching |
| Hiring Manager | Evaluate candidates | Candidate profiles |
| Hiring Manager | Provide feedback | Interview management |
| Administrator | Manage access | Role and permission management |
| Candidate | Submit application | Application management |

# 6. User Permissions — Initial Direction

The following table describes intended access boundaries.

| Capability | Admin | Recruiter | Hiring Manager | Candidate |
|---|---|---|---|---|
| Manage organization | Yes | No | No | No |
| Manage users | Yes | No | No | No |
| Create jobs | Yes | Yes | Configurable | No |
| Edit jobs | Yes | Yes | Configurable | No |
| View candidates | Yes | Yes | Assigned/authorized | Own data only |
| Upload resumes | Yes | Yes | Configurable | Own application |
| Review matching | Yes | Yes | Assigned/authorized | No |
| Manage pipeline | Yes | Yes | Limited | No |
| Schedule interviews | Yes | Yes | Configurable | No |
| Submit interview feedback | Yes | Configurable | Yes | No |
| View organization analytics | Yes | Configurable | Limited | No |

These permissions are a product-level starting point. Exact authorization rules must be defined in the technical access-control specification and enforced by the backend.

A user interface restriction alone is not sufficient authorization.

# 7. User Experience Principles

Niyuvia should provide:

## 7.1 Role-Relevant Interfaces

Each user should see functionality appropriate to their responsibilities.

## 7.2 Clear Navigation

Users should be able to find jobs, candidates, applications and interviews without navigating unnecessary screens.

## 7.3 Understandable Information

Candidate information and AI-generated assessments should use clear terminology.

## 7.4 Meaningful Feedback

The interface should communicate loading, success, empty, validation and error states.

## 7.5 Responsive Experience

Recruiter workflows should work on desktop, tablet and mobile screens.

## 7.6 Accessible Interaction

Forms, navigation and important controls should be designed with accessibility in mind.

# 8. Customer Validation Plan

The personas in this document must be validated through future customer research.

Potential validation activities:

1. Interview recruiters from Indian startups.
2. Interview hiring managers.
3. Understand existing recruitment tools.
4. Identify repetitive recruitment activities.
5. Understand resume review processes.
6. Explore candidate tracking difficulties.
7. Evaluate interest in AI-assisted matching.
8. Test an MVP prototype.
9. Collect usability feedback.
10. Evaluate willingness to adopt or pay.

Customer interviews should focus on actual experiences rather than asking users only whether they like the proposed features.

# 9. Assumptions and Risks

| ID | Assumption / Risk | Validation Approach |
|---|---|---|
| USER-RISK-001 | Recruiters need a centralized workspace | Recruiter interviews |
| USER-RISK-002 | Resume processing can reduce manual work | Workflow testing |
| USER-RISK-003 | Matching explanations are useful | Recruiter usability tests |
| USER-RISK-004 | Hiring managers need structured feedback | Interview workflow testing |
| USER-RISK-005 | SMBs will adopt another recruitment tool | Customer discovery |
| USER-RISK-006 | Candidate information requires strong access controls | Security review |

# 10. MVP User Priorities

The initial MVP should prioritize:

1. Recruiter.
2. Hiring Manager.
3. Organization Administrator.
4. Candidate-facing functionality.

Recruiter workflows should receive the greatest design and implementation attention because they represent the core operational use case.

# 11. Document Governance

This document should be maintained alongside:

- PRD.md
- PRODUCT_VISION.md
- FUNCTIONAL_REQUIREMENTS.md
- USER_STORIES.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Changes to target customers, personas or permissions must be reflected in the relevant product and technical documentation.

---

**Document Status:** Draft — Pending Review

**End of Document**