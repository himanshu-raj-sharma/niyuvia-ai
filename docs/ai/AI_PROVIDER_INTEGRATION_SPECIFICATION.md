# Niyuvia — AI Provider Integration Specification

**Product:** Niyuvia  
**Document:** AI Provider Integration Specification  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines the technical requirements for integrating AI providers into Niyuvia.

It establishes a provider-independent integration architecture that allows AI services to be added, replaced, tested and maintained without modifying core recruitment business logic.

# 2. Integration Principles

All provider integrations must follow these principles:

1. Provider-specific code must remain isolated.
2. Core business services must depend on an internal provider interface.
3. API credentials must remain server-side.
4. All provider responses must be validated.
5. Provider failures must produce normalized errors.
6. External calls must have configurable timeouts.
7. Retries must be bounded and safe.
8. Provider usage and latency must be observable.
9. Candidate information must be shared only when necessary.
10. Initial development must not require paid services.

# 3. Provider Architecture

The AI provider architecture consists of:

- AI Provider Interface.
- Provider Implementations.
- Provider Factory.
- AI Orchestration Service.
- Request and Response Validation.
- Execution Logging.
- Provider Configuration.

Conceptual structure:

    Recruitment Services
            |
            v
    AI Orchestration Service
            |
            v
      AI Provider Interface
            |
       +----+----+
       |         |
       v         v
    Mock       External
    Provider   Provider
                  |
                  v
              AI Service

Business logic must not directly import provider-specific SDKs.

# 4. Initial Provider Strategy

| Environment | Provider | Cost |
|---|---|---|
| Local development | Mock Provider | Free |
| Automated tests | Deterministic Mock Provider | Free |
| Future integration | External LLM API | Usage-based |
| Future semantic search | Embedding Provider | Usage-based |

No paid provider is required during initial development.

# 5. Provider Interface Requirements

Every provider implementation must support the application's required operations.

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

        async def health_check(self):
            ...

The interface must define normalized request and response models.

Provider-specific response formats must not leak into business services.

# 6. Provider Factory

A provider factory must select the implementation based on validated server-side configuration.

Example configuration:

    AI_PROVIDER=mock

Supported initial configuration:

- mock

Future configurations may include approved external providers.

The application must fail safely if an unsupported provider is configured.

# 7. Provider Configuration

Provider settings must be managed through environment variables or an approved secret-management mechanism.

Example future configuration:

    AI_PROVIDER=mock
    AI_REQUEST_TIMEOUT_SECONDS=30
    AI_MAX_RETRIES=2

Future external provider configuration may include:

    AI_API_KEY=
    AI_MODEL=

These variables are examples only. No API key is required for mock development.

Secrets must never be committed to Git or exposed to frontend code.

# 8. Request Processing

Each provider request must follow this sequence:

1. Validate the operation.
2. Validate the input payload.
3. Check authorization and organization access.
4. Select the configured provider.
5. Prepare the provider request.
6. Apply timeout and retry policies.
7. Execute the request.
8. Normalize the response.
9. Validate the response schema.
10. Record execution metadata.
11. Return the validated result.

# 9. Response Normalization

All providers must return data through a common internal response model.

Conceptual response:

    {
      "success": true,
      "provider": "mock",
      "model": null,
      "data": {},
      "usage": null,
      "latency_ms": 0
    }

Provider-specific metadata must be normalized where possible.

Missing provider metadata must remain null rather than being fabricated.

# 10. Error Handling

Provider errors must map to standardized application error codes.

| Provider Condition | Application Error |
|---|---|
| Provider unavailable | AI_PROVIDER_UNAVAILABLE |
| Request timeout | AI_PROVIDER_TIMEOUT |
| Invalid response | AI_INVALID_RESPONSE |
| Schema validation failure | AI_SCHEMA_VALIDATION_FAILED |
| Provider authentication failure | AI_PROVIDER_ERROR |
| Rate limit exceeded | RATE_LIMIT_EXCEEDED |
| Unexpected internal failure | AI_PROCESSING_FAILED |

Sensitive provider error messages must not be exposed directly to users.

# 11. Timeout and Retry Policy

All external requests must use configurable timeouts.

Retry requirements:

- Retry only eligible transient failures.
- Use bounded retry attempts.
- Apply exponential backoff where appropriate.
- Respect provider retry instructions.
- Do not retry permanent validation or authentication failures.
- Prevent duplicate AI executions where possible.

The initial retry configuration must be conservative.

# 12. Provider Health Checks

Providers may expose health information.

Health checks must distinguish:

- Configured.
- Available.
- Unavailable.
- Misconfigured.
- Not yet initialized.

A successful health check must not be interpreted as proof that every AI operation is functioning correctly.

# 13. Usage and Observability

Every external AI execution should record:

- Execution ID.
- Organization ID.
- Operation type.
- Provider identifier.
- Model identifier, when available.
- Prompt version.
- Schema version.
- Request timestamp.
- Response timestamp.
- Latency.
- Token usage, when available.
- Estimated cost, when available.
- Execution status.
- Normalized error code.

Do not log full resume contents or credentials.

# 14. Provider Security

Provider integration must follow these security requirements:

1. Keep credentials on the backend.
2. Use encrypted transport.
3. Restrict provider access to authorized services.
4. Avoid unnecessary candidate data transmission.
5. Validate all provider responses.
6. Treat provider responses as untrusted data.
7. Prevent provider output from triggering unauthorized operations.
8. Review provider retention and data-processing policies before production use.
9. Do not permit candidate resumes to be used for model training by default.

# 15. Mock Provider Requirements

The mock provider is mandatory for the initial development phase.

It must support:

- Deterministic structured responses.
- Resume parsing examples.
- Job requirement analysis examples.
- Interview question generation examples.
- Simulated timeouts.
- Simulated provider failures.
- Invalid response simulation.
- Schema validation testing.

Mock outputs must be clearly identified as simulated.

The mock provider must not present sample results as real candidate assessments.

# 16. External Provider Onboarding

Before integrating an external provider:

1. Review pricing and usage limits.
2. Review privacy and data-processing terms.
3. Review data retention policies.
4. Confirm supported structured output capabilities.
5. Confirm authentication requirements.
6. Configure server-side credentials.
7. Implement the provider adapter.
8. Add provider integration tests.
9. Test timeout and failure behaviour.
10. Review security and privacy implications.

Provider selection must be based on actual requirements, not marketing claims.

# 17. Provider Replacement Strategy

The application must allow a provider to be replaced without changing:

- Recruitment business rules.
- Candidate database models.
- Matching calculations.
- API response contracts.
- Frontend components.

Provider replacement must trigger integration and regression testing.

Historical AI executions must retain their original provider and model metadata.

# 18. Zero-Budget Development Requirements

During the initial MVP:

- Use only the mock provider.
- Do not require paid API keys.
- Do not require paid cloud infrastructure.
- Run provider tests locally.
- Keep the integration interface ready for future expansion.
- Avoid implementing multiple provider adapters before they are needed.

# 19. Future Enhancements

Potential future capabilities:

- Multiple provider support.
- Provider fallback.
- Model selection by task.
- Embedding generation.
- Usage budgets.
- Cost-aware routing.
- Provider performance comparisons.
- Provider-specific monitoring.

These features must be introduced only when justified by actual product requirements.

# 20. Document Governance

This document must remain consistent with:

- AI_ARCHITECTURE.md
- AI_PROMPT_AND_SCHEMA_SPECIFICATION.md
- AI_EVALUATION_AND_TESTING.md
- API_STANDARDS.md
- API_ERROR_CATALOG.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md

Changes to provider contracts, configuration or data-processing behaviour must be reviewed before implementation.

---

**Document Status:** Draft — Pending Review

**End of Document**