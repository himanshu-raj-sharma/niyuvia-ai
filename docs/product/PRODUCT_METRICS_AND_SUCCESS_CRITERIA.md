# Niyuvia — Product Metrics and Success Criteria

**Product:** Niyuvia  
**Document:** Product Metrics and Success Criteria  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 31 October 2026  
**Related Documents:** PRD.md, PRODUCT_VISION.md, MVP_SCOPE_AND_RELEASE_PLAN.md, NON_FUNCTIONAL_REQUIREMENTS.md

---

# 1. Purpose

This document defines the metrics used to evaluate Niyuvia's functionality, usability, AI quality, reliability and product success.

It establishes a measurement framework for the portfolio MVP and provides a foundation for future product validation.

Metrics must be calculated from verifiable data. The system must not display fabricated performance statistics or make unsupported product claims.

# 2. Measurement Principles

Niyuvia metrics must follow these principles:

1. Every metric must have a documented definition.
2. Metrics must be calculated from identifiable data sources.
3. Test data and real customer data must be distinguished.
4. AI quality must be measured independently of AI response generation.
5. Missing data must not be treated as successful performance.
6. Metrics must be reproducible.
7. Product claims must be supported by evidence.
8. Personal candidate information must not be unnecessarily exposed in analytics.

# 3. Metric Categories

Niyuvia uses six primary metric categories:

| Category | Purpose |
|---|---|
| Product Adoption | Understand product usage |
| Recruitment Operations | Measure recruitment workflow activity |
| AI Quality | Evaluate AI-generated information |
| Matching Quality | Evaluate candidate-job alignment |
| System Quality | Measure application reliability and performance |
| Business Validation | Evaluate actual customer value |

# 4. Product Adoption Metrics

## METRIC-PROD-001 — Active Recruiters

**Definition:** Number of distinct authorized recruiters who perform a qualifying product activity during a defined reporting period.

**Measurement:** Count distinct user identifiers with qualifying activity.

**Data Source:** User activity records.

## METRIC-PROD-002 — Jobs Created

**Definition:** Total number of job records created during a reporting period.

**Measurement:** Count jobs by creation timestamp.

**Data Source:** Jobs table.

## METRIC-PROD-003 — Candidates Added

**Definition:** Total number of candidate records created during a reporting period.

**Measurement:** Count candidate records by creation timestamp.

**Data Source:** Candidates table.

## METRIC-PROD-004 — Applications Created

**Definition:** Total number of applications created during a reporting period.

**Measurement:** Count applications by creation timestamp.

**Data Source:** Applications table.

## METRIC-PROD-005 — Workflow Completion Rate

**Definition:** Percentage of initiated workflows that reach their defined successful completion state.

**Formula:**

Completed workflows / Initiated workflows × 100

**Data Source:** Workflow and processing records.

The denominator and completion state must be defined separately for each workflow.

# 5. Recruitment Operations Metrics

## METRIC-REC-001 — Applications per Job

**Definition:** Average number of applications associated with jobs during a reporting period.

**Formula:**

Total applications / Number of jobs in the selected cohort

The job cohort must be clearly identified.

## METRIC-REC-002 — Shortlisting Rate

**Definition:** Percentage of eligible applications that reach the Shortlisted stage.

**Formula:**

Applications reaching Shortlisted / Eligible applications × 100

The measurement cohort and reporting period must be documented.

## METRIC-REC-003 — Interview Conversion Rate

**Definition:** Percentage of eligible applications that reach the Interview Scheduled stage.

**Formula:**

Applications reaching Interview Scheduled / Eligible applications × 100

## METRIC-REC-004 — Offer or Selection Rate

**Definition:** Percentage of eligible applications that reach the Selected stage.

**Formula:**

Applications reaching Selected / Eligible applications × 100

Selection must not be interpreted as a completed hire.

## METRIC-REC-005 — Hiring Conversion Rate

**Definition:** Percentage of eligible applications that reach the Hired stage.

**Formula:**

Applications reaching Hired / Eligible applications × 100

## METRIC-REC-006 — Time to Hire

**Definition:** Elapsed time between the defined start of a recruitment process and the recorded Hired outcome.

The start event must be explicitly defined, such as application creation.

**Formula:**

Hire timestamp − Application creation timestamp

Results should be reported using a documented aggregation method, such as median duration.

## METRIC-REC-007 — Pipeline Distribution

**Definition:** Number of applications currently present in each recruitment stage.

**Data Source:** Current application stage.

Historical stage counts must use stage event history rather than current state alone.

# 6. Resume Processing Metrics

## METRIC-RES-001 — Resume Upload Success Rate

**Definition:** Percentage of attempted uploads successfully stored and associated with candidate records.

**Formula:**

Successful uploads / Valid upload attempts × 100

## METRIC-RES-002 — Text Extraction Success Rate

**Definition:** Percentage of supported resume documents from which usable text is successfully extracted.

**Formula:**

Successful extractions / Eligible extraction attempts × 100

## METRIC-RES-003 — Extraction Processing Time

**Definition:** Time taken to extract text from an uploaded resume.

Measurement should include median and percentile values where sufficient test observations exist.

## METRIC-RES-004 — Resume Processing Failure Rate

**Definition:** Percentage of eligible processing attempts that fail.

**Formula:**

Failed processing attempts / Eligible processing attempts × 100

Failures should be categorized by cause.

# 7. AI Quality Metrics

## METRIC-AI-001 — Field Extraction Precision

**Definition:** Percentage of extracted field values that are correct according to an independently reviewed reference dataset.

**Formula:**

Correct extracted values / Total extracted values × 100

## METRIC-AI-002 — Field Extraction Recall

**Definition:** Percentage of expected reference fields correctly extracted.

**Formula:**

Correctly extracted expected values / Total expected reference values × 100

## METRIC-AI-003 — Structured Output Validity

**Definition:** Percentage of AI responses that pass the required schema validation.

**Formula:**

Schema-valid responses / Total AI responses × 100

## METRIC-AI-004 — Human Correction Rate

**Definition:** Percentage of parsed fields requiring correction during recruiter review.

**Formula:**

Fields corrected / Fields reviewed × 100

A high correction rate indicates a need for further investigation.

## METRIC-AI-005 — AI Processing Latency

**Definition:** Time between an AI processing request and receipt of the provider response.

Provider latency and complete application processing latency must be measured separately.

## METRIC-AI-006 — AI Provider Failure Rate

**Definition:** Percentage of provider requests that fail due to provider errors, timeouts or unavailable services.

## METRIC-AI-007 — AI Cost per Operation

**Definition:** Estimated provider cost associated with one completed AI operation.

This metric is applicable only when actual provider pricing and usage data are available.

Mock provider operations must not be assigned fabricated monetary costs.

# 8. Candidate Matching Quality Metrics

## METRIC-MATCH-001 — Matching Calculation Consistency

**Definition:** Whether identical inputs and scoring configuration versions produce identical results.

**Target:** 100% consistency across deterministic automated test cases.

## METRIC-MATCH-002 — Evidence Coverage

**Definition:** Percentage of evaluated criteria for which supporting or missing evidence is explicitly recorded.

**Formula:**

Criteria with recorded evidence status / Total evaluated criteria × 100

## METRIC-MATCH-003 — Human Review Agreement

**Definition:** Degree of agreement between matching assessments and independently reviewed human assessments.

The evaluation methodology must be documented before reporting a result.

## METRIC-MATCH-004 — Ranking Quality

**Definition:** Quality of candidate ordering compared with an independently established relevance assessment.

Potential evaluation methods include:

- NDCG@K.
- Precision@K.
- Recall@K.

These metrics require a properly designed reference dataset and must not be reported without one.

## METRIC-MATCH-005 — Missing Evidence Rate

**Definition:** Percentage of evaluated criteria for which sufficient candidate information is unavailable.

Missing evidence must not automatically be interpreted as evidence of a skill deficiency.

## METRIC-MATCH-006 — Scoring Reproducibility

**Definition:** Ability to reproduce historical matching results using the recorded input and scoring configuration versions.

# 9. System Quality Metrics

## METRIC-SYS-001 — API Availability

**Definition:** Percentage of monitored API requests that complete successfully according to a documented availability policy.

Local development results must not be represented as a production availability SLA.

## METRIC-SYS-002 — API Latency

**Definition:** Time required for API requests to complete.

Measurement should distinguish:
- Median latency.
- 95th percentile latency.
- 99th percentile latency where sample size supports it.

## METRIC-SYS-003 — API Error Rate

**Definition:** Percentage of requests resulting in application or server errors.

Client validation errors and expected authorization failures should be reported separately from unexpected server failures.

## METRIC-SYS-004 — Automated Test Pass Rate

**Definition:** Percentage of executed automated test cases that pass.

**Formula:**

Passed tests / Executed tests × 100

Skipped and unexecuted tests must be reported separately.

## METRIC-SYS-005 — Critical Defect Count

**Definition:** Number of unresolved defects classified as critical according to the project defect severity policy.

## METRIC-SYS-006 — Security Test Status

**Definition:** Result of required security verification scenarios.

Critical authorization and data-isolation tests must pass before the portfolio MVP is considered ready.

# 10. Business Validation Metrics

These metrics require actual user or customer validation.

## METRIC-BIZ-001 — Recruiter Workflow Satisfaction

Measure recruiter feedback regarding the usability and usefulness of the recruitment workflow.

## METRIC-BIZ-002 — Time Saved per Recruitment Task

Compare the time required to complete a defined recruitment task with and without Niyuvia.

Use a consistent measurement method and representative users.

## METRIC-BIZ-003 — Customer Activation

Measure the percentage of registered organizations that complete the defined activation workflow.

## METRIC-BIZ-004 — Customer Retention

Measure organizations that continue using the product over a defined period.

## METRIC-BIZ-005 — Willingness to Pay

Collect structured feedback about pricing expectations and purchasing intent.

Stated willingness to pay must not be treated as actual revenue.

# 11. MVP Measurement Priorities

The following metrics should receive priority during initial development.

| Metric | MVP Priority |
|---|---|
| Resume upload success | P0 |
| Text extraction success | P0 |
| Structured output validity | P0 |
| Field extraction precision | P0 |
| Matching calculation consistency | P0 |
| Evidence coverage | P0 |
| Application stage accuracy | P0 |
| API error rate | P1 |
| API latency | P1 |
| Automated test pass rate | P0 |
| Dashboard metric accuracy | P0 |
| Recruiter workflow satisfaction | P1 |

# 12. Measurement Dataset

AI and matching quality must be evaluated using a documented reference dataset.

The initial dataset should:

- Use synthetic or appropriately authorized information.
- Include different resume layouts.
- Include PDF and DOCX examples.
- Include incomplete resumes.
- Include ambiguous skill descriptions.
- Include multiple experience levels.
- Include relevant and irrelevant job requirements.
- Have independently reviewed expected outputs.

The dataset must not contain real candidate information without appropriate authorization and safeguards.

# 13. Reporting Requirements

Every metric report should document:

- Metric identifier.
- Metric definition.
- Reporting period.
- Dataset or source.
- Sample size.
- Calculation method.
- Result.
- Known limitations.

Small sample sizes must be disclosed.

AI evaluation results from synthetic data must be identified as synthetic-data evaluation.

# 14. Metrics We Must Not Misrepresent

Niyuvia must not claim:

- Improved hiring quality without suitable outcome evidence.
- Reduced recruitment time without comparative measurements.
- Unbiased AI matching without an appropriate evaluation.
- Production reliability based only on local tests.
- High AI accuracy without a validated reference dataset.
- Customer satisfaction without real user feedback.
- Commercial adoption without actual customer usage.

# 15. MVP Success Criteria

The portfolio MVP will be considered technically successful when:

1. The primary recruitment workflow completes successfully.
2. Resume uploads are validated.
3. Supported resumes can be processed.
4. AI outputs pass structured validation.
5. Human review and correction work.
6. Matching calculations are deterministic.
7. Matching evidence is inspectable.
8. Application stage transitions are validated.
9. Dashboard metrics match stored records.
10. Critical authorization tests pass.
11. Automated test results are documented.
12. Setup instructions allow another developer to run the application.

These are engineering acceptance criteria, not proof of commercial product-market fit.

# 16. Future Product Validation

Before commercial launch, Niyuvia should validate:

- Real recruiter workflow needs.
- Resume processing quality across diverse formats.
- Matching quality with appropriate human-reviewed datasets.
- Product usability.
- Customer retention.
- Willingness to pay.
- Support and operating costs.
- Privacy and security expectations.

# 17. Document Governance

This document must remain consistent with:

- PRD.md
- MVP_SCOPE_AND_RELEASE_PLAN.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- FUNCTIONAL_REQUIREMENTS.md
- REQUIREMENTS_TRACEABILITY_MATRIX.md

Changes to metric definitions must be reviewed to prevent inconsistent reporting.

---

**Document Status:** Draft — Pending Review

**End of Document**