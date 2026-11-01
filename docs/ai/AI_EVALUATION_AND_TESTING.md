# Niyuvia — AI Evaluation and Testing Strategy

**Product:** Niyuvia  
**Document:** AI Evaluation and Testing Strategy  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines the testing and evaluation strategy for all AI-powered capabilities in Niyuvia.

The objective is to ensure that AI components produce valid, reliable, explainable and reproducible outputs before their results are used in recruitment workflows.

# 2. Evaluation Principles

All AI components must follow these principles:

1. AI outputs must be measurable.
2. Testing must use representative sample datasets.
3. Invalid outputs must be detected.
4. Missing information must not be fabricated.
5. Candidate evaluation must remain explainable.
6. AI failures must not be treated as successful processing.
7. Prompt and model changes must trigger appropriate regression tests.
8. Mock provider outputs must be distinguishable from real AI outputs.
9. Human review must remain part of recruitment decisions.
10. Evaluation results must be documented.

# 3. Testing Levels

| Testing Level | Purpose |
|---|---|
| Unit Testing | Test individual AI functions |
| Schema Testing | Validate structured outputs |
| Integration Testing | Test AI services with backend components |
| Regression Testing | Detect unexpected behaviour changes |
| Security Testing | Test prompt injection and data exposure |
| Quality Evaluation | Measure extraction and matching quality |
| End-to-End Testing | Validate complete AI workflows |

# 4. Resume Parsing Evaluation

## 4.1 Objective

Measure the accuracy and consistency of candidate information extracted from resumes.

## 4.2 Test Dataset

Create a controlled dataset containing synthetic resumes.

Include:

- Standard PDF resumes.
- DOCX resumes.
- Resumes with missing fields.
- Resumes with multiple employment records.
- Resumes with different educational formats.
- Resumes containing tables.
- Resumes containing unusual formatting.
- Empty or corrupted documents.
- Documents with misleading embedded instructions.

Do not use real candidate personal information without appropriate authorization and safeguards.

## 4.3 Evaluation Metrics

| Metric | Description |
|---|---|
| Precision | Correct extracted values relative to extracted values |
| Recall | Correct extracted values relative to expected values |
| F1 Score | Harmonic mean of precision and recall |
| Field Accuracy | Accuracy for individual fields |
| Extraction Failure Rate | Percentage of failed extractions |

Evaluate fields independently, including:

- Name.
- Email.
- Phone.
- Skills.
- Experience.
- Education.
- Certifications.
- Projects.

# 5. Job Requirement Analysis Evaluation

Evaluate whether the system correctly extracts:

- Required skills.
- Preferred skills.
- Responsibilities.
- Experience requirements.
- Education requirements.
- Certifications.
- Domain requirements.

Test ambiguous job descriptions and descriptions containing contradictory requirements.

Measure:

- Requirement extraction precision.
- Requirement extraction recall.
- Classification accuracy.
- Ambiguity detection accuracy.

Recruiter-approved requirements must be treated as the authoritative matching input.

# 6. Candidate Matching Evaluation

## 6.1 Objective

Determine whether matching scores and explanations accurately reflect documented candidate evidence and approved job requirements.

## 6.2 Evaluation Dataset

Create synthetic candidate profiles and approved job descriptions.

Include:

- Strong skill alignment.
- Partial skill alignment.
- Missing skill evidence.
- Relevant project experience.
- Irrelevant project experience.
- Different experience levels.
- Incomplete resumes.
- Contradictory candidate information.

## 6.3 Metrics

| Metric | Purpose |
|---|---|
| NDCG@K | Evaluate ranking quality |
| Precision@K | Measure relevance among top-ranked candidates |
| Recall@K | Measure relevant candidates retrieved |
| Recruiter Agreement | Compare system outputs with reviewed assessments |
| Evidence Coverage | Measure supported evaluation requirements |
| Score Reproducibility | Verify identical inputs produce identical results |

Matching scores must not be interpreted as hiring probabilities.

# 7. Evidence Quality Evaluation

Every generated evaluation explanation must be checked for factual support.

Tests must verify:

- Evidence refers to the correct candidate.
- Evidence comes from available candidate information.
- Evidence supports the stated requirement.
- Unsupported statements are rejected.
- Missing evidence is clearly identified.
- Contradictory information is not silently ignored.

The system must distinguish verified evidence from AI-generated interpretations.

# 8. Structured Output Validation

Every AI response must be tested against its expected schema.

Test cases must include:

- Valid JSON.
- Missing required fields.
- Invalid data types.
- Unexpected fields.
- Invalid enum values.
- Excessively long strings.
- Oversized arrays.
- Null values.
- Malformed JSON.
- Inconsistent nested objects.

Invalid responses must never be persisted as successful processing results.

# 9. Prompt Injection and Security Testing

Security tests must include:

- Instructions embedded in resumes.
- Instructions embedded in job descriptions.
- Attempts to reveal system prompts.
- Attempts to expose API credentials.
- Attempts to manipulate matching scores.
- Attempts to trigger unauthorized operations.
- Maliciously formatted document content.

The expected result is that untrusted document content cannot override system instructions or authorize application actions.

# 10. AI Provider Testing

The provider abstraction must be tested independently of external AI services.

Test scenarios:

- Successful structured response.
- Provider timeout.
- Provider unavailable.
- Invalid response.
- Rate limiting.
- Authentication failure.
- Network interruption.
- Malformed output.
- Retry behaviour.

The mock provider must support deterministic test responses and simulated failures.

# 11. Regression Testing

A stable evaluation dataset must be maintained.

Whenever any of the following changes, relevant regression tests must run:

- Prompt instructions.
- Output schemas.
- Provider integration.
- Model version.
- Matching weights.
- Extraction logic.
- Validation rules.
- Document processing logic.

Historical evaluation results must remain traceable to their original versions.

# 12. Human Review

Recruiters must be able to review AI-generated information.

Review activities include:

- Correcting extracted candidate information.
- Approving job requirements.
- Reviewing matching explanations.
- Reporting incorrect AI outputs.
- Providing feedback on candidate relevance.

Human decisions must not be silently overwritten by subsequent AI processing.

# 13. Zero-Cost Testing Strategy

During initial development:

- Use synthetic test datasets.
- Use a deterministic mock AI provider.
- Use local automated tests.
- Avoid paid API dependencies.
- Test parsing and validation independently.
- Store sample expected outputs.
- Simulate provider failures.

No paid AI service is required to implement the initial testing framework.

# 14. Initial Quality Gates

The following are proposed development targets, not measured performance claims.

| Area | Initial Target |
|---|---|
| Schema validation | 100% of accepted outputs pass validation |
| Deterministic matching | 100% reproducibility for identical inputs and configuration |
| Invalid response handling | No malformed output accepted as successful |
| Prompt injection tests | No unauthorized instruction execution in test cases |
| AI failure handling | All simulated failures produce identifiable states |
| Test coverage | Critical AI paths covered by automated tests |

Resume extraction and matching accuracy targets must be established after collecting baseline results from the evaluation dataset.

# 15. Evaluation Reports

Each formal evaluation run should record:

- Evaluation identifier.
- Dataset version.
- Prompt version.
- Schema version.
- Provider and model.
- Test execution date.
- Metric results.
- Failed test cases.
- Known limitations.
- Reviewer approval.

# 16. Release Readiness

AI functionality must not be considered ready merely because it returns a successful response.

Before release, verify:

1. Schema validation works.
2. Failure handling works.
3. Security tests pass.
4. Regression tests pass.
5. Evidence quality has been reviewed.
6. Sensitive data handling has been reviewed.
7. Human review workflows work.
8. Known limitations are documented.

# 17. Future Improvements

Future evaluation capabilities may include:

- Larger benchmark datasets.
- Automated evaluation pipelines.
- Human feedback analysis.
- Fairness evaluation.
- Provider comparison.
- Model performance monitoring.
- Cost and latency analysis.
- Advanced ranking evaluation.

These enhancements must be introduced based on actual product requirements and available evidence.

# 18. Document Governance

This document must remain consistent with:

- AI_ARCHITECTURE.md
- AI_PROMPT_AND_SCHEMA_SPECIFICATION.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- BUSINESS_RULES.md

Changes to evaluation criteria must be documented and reviewed.

---

**Document Status:** Draft — Pending Review

**End of Document**