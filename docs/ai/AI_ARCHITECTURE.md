# Niyuvia — AI Architecture Specification

**Product:** Niyuvia  
**Document:** AI Architecture Specification  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines the AI architecture, processing components, provider integrations, validation requirements and responsible AI principles for Niyuvia.

The architecture must support:

- Resume information extraction.
- Job requirement analysis.
- Candidate-to-job matching.
- Evidence-based candidate evaluations.
- Interview question generation.
- Future AI-assisted recruitment workflows.

The architecture must remain modular, testable, cost-conscious and provider-independent.

# 2. Architectural Principles

Niyuvia AI must follow these principles:

1. AI functionality must be separated from core recruitment business logic.
2. AI providers must be accessed through a common abstraction layer.
3. All AI outputs must be validated before application use.
4. AI-generated information must remain distinguishable from verified information.
5. Candidate evaluations must be explainable and traceable.
6. AI must not independently make final hiring decisions.
7. AI failures must not silently produce fabricated results.
8. Candidate data must be processed with appropriate privacy and security safeguards.
9. Development must support a zero-cost mock provider.
10. Future provider integrations must not require rewriting business modules.

# 3. High-Level Architecture

The AI architecture consists of the following components:

- AI Orchestration Layer.
- AI Provider Interface.
- Resume Parsing Service.
- Job Requirement Analysis Service.
- Candidate Matching Engine.
- Interview Question Generation Service.
- AI Output Validation Layer.
- AI Execution and Audit Records.

Conceptual processing flow:

    Frontend
       |
       v
    FastAPI Backend
       |
       v
    AI Orchestration Layer
       |
       +-----------------------+
       |                       |
       v                       v
    Resume Parser          Job Analyzer
       |                       |
       +-----------+-----------+
                   |
                   v
            Matching Engine
                   |
                   v
           Evaluation Output
                   |
                   v
          Validation Layer
                   |
                   v
          PostgreSQL Database

# 4. AI Provider Abstraction

All external AI providers must be accessed through a common interface.

The application must not directly depend on a particular provider's SDK throughout its business logic.

## 4.1 Provider Interface

The interface should support:

- Structured text generation.
- Schema-constrained output.
- Optional embedding generation.
- Provider health checks.
- Provider error classification.

Conceptual interface:

    class AIProvider:
        async def generate_structured(
            self,
            prompt: str,
            response_schema: dict
        ):
            ...

        async def generate_embedding(
            self,
            text: str
        ):
            ...

The actual implementation must use validated input and output models.

## 4.2 Initial Provider Strategy

| Environment | Provider |
|---|---|
| Local development | Mock AI Provider |
| Automated tests | Deterministic Mock Provider |
| Future integration | External LLM API |
| Future semantic matching | Embedding provider |

The initial implementation must not require paid API credentials.

# 5. AI Orchestration Layer

The orchestration layer coordinates AI-related operations.

Responsibilities:

- Select the configured provider.
- Validate incoming requests.
- Execute the appropriate AI service.
- Validate returned output.
- Record execution metadata.
- Handle provider failures.
- Return normalized results.

The orchestration layer must not contain all resume parsing, matching and interview generation logic in one large function.

Each capability must remain independently testable.

# 6. Resume Parsing Service

## 6.1 Objective

Extract structured candidate information from uploaded resumes.

Supported initial formats:

- PDF.
- DOCX.

## 6.2 Extraction Pipeline

1. Receive an authorized resume-processing request.
2. Retrieve the uploaded document from private storage.
3. Validate file type and size.
4. Perform malware and file safety checks.
5. Extract document text.
6. Normalize extracted text.
7. Send sanitized text to the configured AI provider.
8. Validate the structured response.
9. Store extraction results and provenance.
10. Make extracted information available for recruiter review.

## 6.3 Initial Extraction Fields

- Full name.
- Email address.
- Phone number.
- Location.
- Professional summary.
- Technical skills.
- Work experience.
- Education.
- Certifications.
- Projects.

Missing information must remain null or explicitly marked as unavailable.

The system must not invent missing qualifications, experience or skills.

## 6.4 Extraction Provenance

Extracted fields should retain information about:

- Source resume.
- Extraction timestamp.
- Provider.
- Model identifier, when applicable.
- Prompt version.
- Schema version.
- Validation status.

Recruiters must be able to review and correct extracted information.

# 7. Job Requirement Analysis Service

## 7.1 Objective

Convert recruiter-provided job descriptions into structured, reviewable requirements.

## 7.2 Requirement Categories

- Required skills.
- Preferred skills.
- Relevant experience.
- Responsibilities.
- Education requirements.
- Certifications.
- Domain knowledge.

## 7.3 Processing Rules

- Preserve the original job description.
- Extract requirements into a structured schema.
- Distinguish mandatory and preferred requirements.
- Do not silently invent requirements.
- Require recruiter approval before using extracted requirements for matching.
- Version approved requirements.

# 8. Candidate Matching Engine

## 8.1 Objective

Evaluate how closely a candidate's documented qualifications align with an approved job requirement profile.

The matching engine must provide evidence and explanations rather than only a numerical score.

## 8.2 Initial Scoring Model

| Evaluation Dimension | Initial Weight |
|---|---:|
| Required skills | 45% |
| Preferred skills | 15% |
| Relevant experience | 15% |
| Project and responsibility evidence | 25% |
| Total | 100% |

These weights are initial product assumptions and must be reviewed using recruiter feedback and evaluation testing.

## 8.3 Matching Output

The output should contain:

- Overall alignment score.
- Individual dimension scores.
- Matched requirements.
- Missing or unverified requirements.
- Supporting resume evidence.
- Evidence coverage.
- Evaluation version.
- Evaluation timestamp.

## 8.4 Important Scoring Rules

- Missing information must not automatically mean a candidate lacks a skill.
- Evidence coverage must be distinguishable from alignment score.
- Scoring must use approved job requirements.
- Identical inputs and configuration versions must produce reproducible results.
- Score changes must be versioned.
- The score must not be represented as a probability of hiring success.

## 8.5 Human Decision Requirement

The matching engine must never automatically reject candidates.

Recruiters retain responsibility for reviewing candidates and making hiring decisions.

# 9. Interview Question Generation

The interview question generation service may generate:

- Technical questions.
- Role-specific questions.
- Experience-based questions.
- Project discussion questions.
- Behavioural questions.
- Evaluation rubrics.

Generated questions must be relevant to the approved job requirements.

AI-generated questions are suggestions, not verified assessments of candidate ability.

Interviewers must record their own observations and feedback.

# 10. AI Output Validation

Every structured AI response must pass validation before it is accepted.

Validation must include:

- Schema validation.
- Required field checks.
- Data type checks.
- Allowed value checks.
- Length limits.
- Evidence reference validation.
- Invalid or contradictory output detection.

Invalid output must produce an identifiable failure state.

The system must not silently convert malformed output into a successful evaluation.

# 11. Responsible AI and Fairness

Niyuvia must use AI as decision support rather than autonomous hiring authority.

The system must not use protected characteristics or inappropriate proxies to rank candidates.

Prohibited evaluation inputs include:

- Religion.
- Caste.
- Race.
- Gender.
- Sexual orientation.
- Disability.
- Political affiliation.
- Other protected or irrelevant personal characteristics.

Candidate photographs and other irrelevant personal attributes must not influence matching scores.

The system must support recruiter review and correction.

# 12. Prompt Security

Resume documents and job descriptions must be treated as untrusted input.

The AI system must:

- Never follow instructions embedded inside uploaded resumes.
- Separate system instructions from document content.
- Prevent document text from changing application behaviour.
- Avoid exposing internal prompts and credentials.
- Validate all generated structured output.
- Restrict AI operations to their authorized purpose.

# 13. AI Execution Records

Every AI execution should record:

| Field | Description |
|---|---|
| run_id | Unique execution identifier |
| organization_id | Owning organization |
| operation_type | Parsing, analysis, matching or generation |
| provider | Selected provider |
| model | Model identifier, if applicable |
| prompt_version | Prompt version |
| schema_version | Output schema version |
| status | Pending, processing, completed or failed |
| latency_ms | Execution duration |
| token_usage | Usage metadata, when available |
| error_code | Normalized failure code |
| created_at | UTC creation timestamp |

Sensitive resume content must not be unnecessarily duplicated in execution logs.

# 14. Failure Handling

AI failures must be handled explicitly.

Possible failure conditions:

- Provider unavailable.
- Provider timeout.
- Invalid provider response.
- Schema validation failure.
- Document extraction failure.
- Unsupported document format.
- Processing interruption.

The application must preserve the original resume and avoid creating misleading evaluation records after a failed operation.

Retry operations must be bounded and safe.

# 15. Zero-Cost Development Strategy

The initial development environment must work without paid AI services.

The mock provider must:

- Return deterministic structured responses.
- Support resume parsing examples.
- Support job requirement examples.
- Support interview question examples.
- Simulate provider failures.
- Simulate malformed responses.
- Support automated tests.

Mock responses must be clearly labelled as simulated data.

They must never be presented as genuine AI-generated candidate assessments.

# 16. Evaluation and Quality Metrics

AI functionality must be evaluated before being considered reliable.

| Component | Evaluation Metric |
|---|---|
| Resume parsing | Field-level precision and recall |
| Skill extraction | Precision, recall and F1 |
| Job analysis | Requirement extraction accuracy |
| Candidate matching | NDCG@K and recruiter agreement |
| Evidence generation | Evidence relevance and factual consistency |
| Structured output | Schema validation success rate |
| Provider integration | Latency and failure rate |

Evaluation results must be measured against reviewed sample datasets.

# 17. Future Enhancements

The following capabilities are outside the initial MVP:

- Semantic candidate search.
- Embedding-based retrieval.
- Advanced recruiter assistant.
- AI-generated interview summaries.
- Multi-provider routing.
- Advanced evaluation calibration.
- Recruitment analytics summaries.
- Agentic workflow automation.

These features must be introduced only after the foundational AI architecture is tested.

# 18. Document Governance

This document must remain consistent with:

- PRD.md
- FUNCTIONAL_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md
- BUSINESS_RULES.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- DATABASE_DESIGN.md
- API_STANDARDS.md

Major changes to AI scoring, provider behaviour, candidate evaluation or data processing must be documented and reviewed.

---

**Document Status:** Draft — Pending Review

**End of Document**