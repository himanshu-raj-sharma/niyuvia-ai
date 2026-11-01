# Niyuvia — Product Assumptions, Constraints and Risks

**Product:** Niyuvia  
**Document:** Product Assumptions, Constraints and Risks  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 31 October 2026

---

# 1. Purpose

This document identifies the assumptions, constraints, dependencies and risks that may affect Niyuvia's product development, technical implementation, user adoption and future commercial launch.

It provides a structured approach to identifying uncertainties before they become development or business problems.

# 2. Product Assumptions

| ID | Assumption | Validation Approach | Status |
|---|---|---|---|
| PA-001 | Indian SMBs need a centralized recruitment workspace | Interview recruiters and hiring managers | Unvalidated |
| PA-002 | Recruiters spend time manually reviewing resumes | Conduct workflow interviews | Unvalidated |
| PA-003 | Structured resume information can improve recruitment workflows | Test with synthetic datasets and user feedback | Unvalidated |
| PA-004 | Evidence-backed matching can assist candidate review | Conduct human evaluation | Unvalidated |
| PA-005 | SMBs prefer a relatively simple ATS over complex enterprise HR platforms | Customer discovery | Unvalidated |
| PA-006 | Recruiters value transparent matching explanations | Usability testing | Unvalidated |
| PA-007 | An API-based AI integration is sufficient for the initial product | Technical evaluation | Partially validated |
| PA-008 | A local-first MVP can demonstrate the primary product workflows | End-to-end development testing | Planned |

These assumptions must not be represented as established market facts until validated.

# 3. Technical Constraints

## TC-001 — Zero-Budget Development

Initial development must not require paid infrastructure, subscriptions or mandatory external services.

## TC-002 — Single Primary Developer

The initial implementation is planned around one primary developer.

Architecture and documentation must remain understandable and maintainable by another engineer.

## TC-003 — Local-First Development

The MVP must be fully testable on a local development environment.

## TC-004 — Limited AI Access

External AI provider availability and free-tier usage may be limited.

A mock provider must support development and automated tests.

## TC-005 — Limited Development Time

The product must be developed alongside job preparation and applications.

## TC-006 — Portfolio MVP Scope

The initial release must prioritize core recruitment workflows rather than advanced commercial functionality.

# 4. Product Constraints

## PC-001 — Recruitment Focus

Niyuvia is an ATS and recruitment management platform, not a complete HRMS.

## PC-002 — Human Hiring Authority

Recruiters and hiring managers retain control over hiring decisions.

## PC-003 — Explainable Matching

Candidate assessments must expose supporting evidence and limitations.

## PC-004 — Privacy-Aware Processing

Candidate information must be handled with appropriate privacy and security controls.

## PC-005 — India-First Design

Initial workflows should account for relevant Indian recruitment practices without assuming every organization has identical requirements.

# 5. Technical Dependencies

| ID | Dependency | Purpose | Fallback |
|---|---|---|---|
| DEP-001 | Node.js | Frontend development | Supported local installation |
| DEP-002 | Python | Backend development | Supported local installation |
| DEP-003 | PostgreSQL | Persistent application data | Local PostgreSQL |
| DEP-004 | Resume extraction libraries | PDF and DOCX processing | Manual candidate entry |
| DEP-005 | AI provider | Optional structured extraction | Mock provider |
| DEP-006 | Git | Version control | Local Git repository |
| DEP-007 | Docker | Reproducible local environment | Native local installation |

External services must not become mandatory dependencies for unrelated application workflows.

# 6. Product and Market Risks

| Risk ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| RISK-001 | SMB recruitment needs differ from assumptions | Medium | High | Interview target users |
| RISK-002 | Existing ATS products already satisfy the target market | High | High | Competitive research and differentiation |
| RISK-003 | Recruiters may not trust AI-generated assessments | Medium | High | Evidence-based explanations and human review |
| RISK-004 | Customers may not be willing to pay | Medium | High | Validate willingness to pay |
| RISK-005 | Product scope becomes too large | High | High | Enforce MVP scope |
| RISK-006 | Limited developer time delays implementation | High | Medium | Prioritize job search and core features |
| RISK-007 | Free service limitations affect demonstrations | Medium | Medium | Local-first fallback architecture |

# 7. Technical Risks

| Risk ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| RISK-008 | AI extraction produces inaccurate data | High | High | Schema validation and human correction |
| RISK-009 | Resume files contain malicious or malformed content | Medium | High | File validation and isolated processing |
| RISK-010 | Cross-organization data exposure | Medium | Critical | Backend authorization and tenant-isolation tests |
| RISK-011 | External AI provider becomes unavailable | Medium | Medium | Mock provider and failure handling |
| RISK-012 | Database schema changes break existing workflows | Medium | High | Versioned migrations and regression tests |
| RISK-013 | Matching scores are misleading | Medium | High | Documented scoring and evidence coverage |
| RISK-014 | Poorly structured code becomes difficult to maintain | Medium | High | Modular architecture and code review |
| RISK-015 | Secrets are accidentally committed | Medium | Critical | Git exclusions and secret scanning |

# 8. AI and Recruitment Risks

## RISK-016 — Biased Candidate Evaluation

AI-assisted evaluation may produce inconsistent or discriminatory outcomes.

Mitigation:
- Exclude protected characteristics and inappropriate proxies.
- Use job-related criteria.
- Evaluate matching quality.
- Preserve human review.
- Document limitations.

## RISK-017 — Missing Resume Information

A resume may not contain sufficient evidence for every requirement.

Mitigation:
- Represent missing information explicitly.
- Avoid treating missing evidence as proof of inability.
- Allow recruiter review.

## RISK-018 — Prompt Injection

Uploaded documents may contain malicious instructions intended to influence AI processing.

Mitigation:
- Treat document contents as untrusted data.
- Separate system instructions from document content.
- Restrict AI tools and operations.
- Validate structured outputs.

## RISK-019 — Overreliance on Matching Scores

Users may interpret alignment scores as definitive candidate quality rankings.

Mitigation:
- Explain scoring dimensions.
- Display evidence.
- Display missing information.
- State that scores are decision-support signals.

# 9. Privacy and Legal Risks

## RISK-020 — Candidate Data Exposure

Unauthorized access may expose personal information.

Mitigation:
- Backend authorization.
- Organization isolation.
- Private resume storage.
- Security testing.

## RISK-021 — Inappropriate Data Retention

Candidate information may be retained longer than justified.

Mitigation:
- Define retention policies.
- Implement controlled deletion workflows.
- Review backup retention.

## RISK-022 — External Provider Data Processing

Candidate information may be shared with third-party AI or infrastructure providers.

Mitigation:
- Review provider terms.
- Minimize transferred information.
- Document processing purposes.
- Apply appropriate privacy safeguards.

Applicable Indian privacy and other legal obligations must be reviewed before commercial deployment.

# 10. Risk Severity Definitions

| Severity | Meaning |
|---|---|
| Critical | Could cause serious security, privacy or operational harm |
| High | Could significantly affect product success or data integrity |
| Medium | Could delay development or reduce usability |
| Low | Limited impact with straightforward mitigation |

# 11. Risk Management Process

For each identified risk:

1. Assign a unique risk identifier.
2. Document its cause and potential impact.
3. Estimate likelihood and severity.
4. Define a mitigation strategy.
5. Assign an owner when a team exists.
6. Review its status periodically.
7. Record whether the mitigation was effective.

# 12. Open Product Decisions

The following decisions require validation or implementation review:

| Decision ID | Decision | Current Position |
|---|---|---|
| DEC-001 | Initial authentication provider | To be evaluated |
| DEC-002 | Resume storage abstraction | Local private storage initially |
| DEC-003 | AI provider | Provider-independent interface |
| DEC-004 | Candidate duplicate policy | Warn before merging |
| DEC-005 | Matching scoring configuration | Initial documented weights |
| DEC-006 | Background processing | Lightweight MVP approach |
| DEC-007 | Commercial pricing | Deferred until validation |
| DEC-008 | Multi-tenant production isolation | Required before commercial launch |

# 13. Risk Acceptance Policy

Critical security and privacy risks must not be accepted merely to meet development deadlines.

Temporary MVP limitations must be documented and must not expose real candidate information to unsafe processing.

Commercial launch requires a separate review of unresolved high-impact risks.

# 14. Document Governance

This document must be reviewed when:

- MVP scope changes.
- New external services are introduced.
- AI capabilities change.
- Candidate data processing changes.
- Security architecture changes.
- Product validation reveals incorrect assumptions.

---

**Document Status:** Draft — Pending Review

**End of Document**