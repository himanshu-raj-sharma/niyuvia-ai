# Niyuvia — Requirements Traceability Matrix

**Product:** Niyuvia  
**Document:** Requirements Traceability Matrix (RTM)  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 31 October 2026

---

# 1. Purpose

This document establishes traceability between Niyuvia's product requirements, functional requirements, user stories, user workflows and verification activities.

The objective is to ensure that every approved requirement has:

- A documented business purpose.
- A corresponding functional requirement where applicable.
- A relevant user story.
- An associated workflow where applicable.
- A defined verification method.

This matrix supports development planning, quality assurance, change management and release readiness.

# 2. Traceability Structure

The following relationship is used:

**Product Requirement → Functional Requirement → User Story → Workflow → Test Case**

Not every requirement requires a separate user workflow. Technical, security and infrastructure requirements may instead be verified through automated tests, architecture reviews or security assessments.

# 3. Requirement Traceability

| Feature Area | Functional Requirement | User Story | Workflow | Verification |
|---|---|---|---|---|
| Authentication | FR-AUTH-001 | US-AUTH-001 | WF-AUTH-001 | TC-AUTH-001 |
| Logout | FR-AUTH-002 | US-AUTH-002 | WF-AUTH-001 | TC-AUTH-002 |
| Role permissions | FR-AUTH-004 | US-AUTH-003 | WF-AUTH-001 | TC-AUTH-003 |
| Organization profile | FR-ORG-001 | US-ORG-001 | Organization workflow | TC-ORG-001 |
| Organization membership | FR-ORG-002 | US-ORG-002 | Organization workflow | TC-ORG-002 |
| Job creation | FR-JOB-001 | US-JOB-001 | WF-JOB-001 | TC-JOB-001 |
| Job listing | FR-JOB-002 | US-JOB-002 | WF-JOB-001 | TC-JOB-002 |
| Job updates | FR-JOB-004 | US-JOB-003 | WF-JOB-001 | TC-JOB-003 |
| Job status | FR-JOB-005 | US-JOB-004 | WF-JOB-001 | TC-JOB-004 |
| Candidate creation | FR-CAND-001 | US-CAND-001 | WF-CAND-001 | TC-CAND-001 |
| Candidate search | FR-CAND-002 | US-CAND-002 | WF-CAND-001 | TC-CAND-002 |
| Candidate profile | FR-CAND-003 | US-CAND-003 | WF-CAND-001 | TC-CAND-003 |
| Candidate updates | FR-CAND-004 | US-CAND-004 | WF-CAND-001 | TC-CAND-004 |
| Duplicate detection | FR-CAND-005 | US-CAND-005 | WF-CAND-001 | TC-CAND-005 |
| Resume upload | FR-RES-001 | US-RES-001 | WF-RES-001 | TC-RES-001 |
| Secure resume storage | FR-RES-002 | US-RES-004 | WF-RES-001 | TC-RES-002 |
| Resume extraction | FR-RES-003 | US-RES-002 | WF-RES-002 | TC-RES-003 |
| Processing status | FR-RES-004 | US-RES-003 | WF-RES-002 | TC-RES-004 |
| Resume parsing | FR-AI-001 | US-AI-001 | WF-AI-001 | TC-AI-001 |
| AI schema validation | FR-AI-002 | US-AI-001 | WF-AI-001 | TC-AI-002 |
| Human review | FR-AI-003 | US-AI-002 | WF-AI-001 | TC-AI-003 |
| AI metadata | FR-AI-004 | US-AI-003 | WF-AI-001 | TC-AI-004 |
| Job requirement analysis | FR-JOB-AI-001 | US-REQ-001 | WF-AI-002 | TC-REQ-001 |
| Job requirement approval | FR-JOB-AI-002 | US-REQ-002 | WF-AI-002 | TC-REQ-002 |
| Matching calculation | FR-MATCH-001 | US-MATCH-001 | WF-MATCH-001 | TC-MATCH-001 |
| Matching evidence | FR-MATCH-002 | US-MATCH-002 | WF-MATCH-001 | TC-MATCH-002 |
| Missing evidence | FR-MATCH-003 | US-MATCH-003 | WF-MATCH-001 | TC-MATCH-003 |
| Evaluation history | FR-MATCH-005 | US-MATCH-004 | WF-MATCH-001 | TC-MATCH-004 |
| Human hiring decision | FR-MATCH-006 | US-MATCH-005 | WF-MATCH-001 | TC-MATCH-005 |
| Application creation | FR-APP-001 | US-APP-001 | WF-APP-001 | TC-APP-001 |
| Application listing | FR-APP-003 | US-APP-002 | WF-APP-001 | TC-APP-002 |
| Application history | FR-APP-004 | US-APP-003 | WF-APP-001 | TC-APP-003 |
| Pipeline stages | FR-PIPE-001 | US-PIPE-001 | WF-PIPE-001 | TC-PIPE-001 |
| Stage transitions | FR-PIPE-002 | US-PIPE-001 | WF-PIPE-001 | TC-PIPE-002 |
| Stage history | FR-PIPE-003 | US-PIPE-002 | WF-PIPE-001 | TC-PIPE-003 |
| Pipeline filtering | FR-PIPE-004 | US-PIPE-003 | WF-PIPE-001 | TC-PIPE-004 |
| Interview scheduling | FR-INT-001 | US-INT-001 | WF-INT-001 | TC-INT-001 |
| Interview feedback | FR-INT-004 | US-INT-002 | WF-INT-002 | TC-INT-002 |
| Dashboard overview | FR-DASH-001 | US-DASH-001 | WF-DASH-001 | TC-DASH-001 |
| Recruitment funnel | FR-DASH-002 | US-DASH-002 | WF-DASH-001 | TC-DASH-002 |
| Candidate privacy | FR-PRIV-001 | US-PRIV-001 | WF-PRIV-001 | TC-PRIV-001 |
| Candidate correction | FR-PRIV-002 | US-PRIV-002 | WF-PRIV-001 | TC-PRIV-002 |
| Data deletion | FR-PRIV-003 | US-PRIV-003 | WF-PRIV-002 | TC-PRIV-003 |

**Note:** The workflow names for organization management and some candidate privacy activities are logical placeholders until those workflows receive dedicated specifications. Test IDs identify planned tests, not tests already implemented.

# 4. Non-Functional Requirement Traceability

| Requirement Category | Requirement IDs | Verification Approach |
|---|---|---|
| Performance | NFR-PERF-001 to NFR-PERF-005 | Performance tests |
| Security | NFR-SEC-001 to NFR-SEC-008 | Security review and tests |
| Privacy | NFR-PRIV-001 to NFR-PRIV-006 | Privacy and data-access review |
| Reliability | NFR-REL-001 to NFR-REL-005 | Failure and integration tests |
| Scalability | NFR-SCALE-001 to NFR-SCALE-004 | Architecture review |
| Maintainability | NFR-MAINT-001 to NFR-MAINT-005 | Code review and static analysis |
| Usability | NFR-USE-001 to NFR-USE-004 | Usability testing |
| Accessibility | NFR-ACC-001 to NFR-ACC-004 | Accessibility testing |
| Compatibility | NFR-COMP-001 to NFR-COMP-003 | Browser and API testing |
| Observability | NFR-OBS-001 to NFR-OBS-004 | Logging verification |
| Testing | NFR-TEST-001 to NFR-TEST-005 | Automated test reports |
| Cost | NFR-COST-001 to NFR-COST-003 | Development environment review |
| Deployment | NFR-DEP-001 to NFR-DEP-004 | Environment and deployment verification |

# 5. MVP Release Traceability

The following table identifies the major MVP deliverables and their corresponding requirements.

| MVP Deliverable | Required Areas | Release Priority |
|---|---|---|
| Authentication foundation | AUTH, SEC | P0 |
| Job management | JOB | P0 |
| Candidate management | CAND | P0 |
| Resume upload and extraction | RES | P0 |
| AI-assisted parsing | AI | P0 |
| Job requirement analysis | JOB-AI | P0 |
| Candidate-job matching | MATCH | P0 |
| Application management | APP | P0 |
| Recruitment pipeline | PIPE | P0 |
| Interview management | INT | P1 |
| Dashboard and analytics | DASH | P1 |
| Quality and privacy verification | PRIV, SEC, TEST | P0 |

# 6. Traceability Status Definitions

| Status | Meaning |
|---|---|
| Not Started | Implementation has not begun |
| In Progress | Implementation is underway |
| Implemented | Development is complete |
| Verified | Acceptance criteria and relevant tests have passed |
| Deferred | Approved for a future release |
| Blocked | Cannot proceed due to an unresolved dependency |

All requirements should initially be marked Not Started in the implementation tracker.

A requirement must not be marked Verified solely because its documentation exists.

# 7. Change Impact Analysis

Whenever a requirement changes, the following areas must be reviewed:

1. Product scope.
2. Functional requirements.
3. User stories.
4. User workflows.
5. Database entities and relationships.
6. API contracts.
7. Frontend components.
8. Backend services.
9. Automated tests.
10. Security and privacy controls.

Changes to matching logic must additionally trigger review of scoring configuration, evaluation history and AI quality tests.

# 8. Quality Assurance Responsibilities

During implementation, the development and QA process must ensure:

- Every P0 functional requirement has a verification method.
- Critical authorization requirements have negative tests.
- Important state transitions have valid and invalid transition tests.
- AI outputs have schema validation tests.
- Matching calculations have deterministic test cases.
- Privacy-sensitive operations have access-control tests.
- Failed operations do not create inconsistent records.

# 9. Release Approval Criteria

Before a portfolio MVP release:

1. Required MVP features must be implemented.
2. Relevant P0 acceptance criteria must pass.
3. Critical authorization tests must pass.
4. Core recruitment workflow tests must pass.
5. No development secrets may be committed.
6. Known limitations must be documented.
7. The README and setup instructions must be accurate.
8. The demonstration must use synthetic candidate data.

# 10. Document Governance

This matrix must be updated whenever requirements, user stories, workflows or test plans change.

It is the central reference for checking whether product requirements are implemented and verified.

---

**Document Status:** Draft — Pending Review

**End of Document**