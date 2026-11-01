# Niyuvia — AI Observability and Cost Management

**Product:** Niyuvia  
**Document:** AI Observability and Cost Management Specification  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines how Niyuvia monitors, measures and manages AI-related operations.

The objectives are to:

- Monitor AI execution reliability.
- Identify processing failures.
- Measure response latency.
- Track token consumption.
- Estimate external provider costs.
- Detect unusual usage patterns.
- Support operational troubleshooting.
- Establish a foundation for future usage limits.

# 2. Scope

This specification covers:

1. AI execution monitoring.
2. Structured execution logging.
3. Performance metrics.
4. Provider health monitoring.
5. Token usage tracking.
6. Cost estimation.
7. Error monitoring.
8. Usage limits.
9. Operational reporting.
10. Future cost optimization.

# 3. Observability Architecture

AI observability must integrate with the existing AI orchestration layer.

Conceptual architecture:

    AI Request
        |
        v
    AI Orchestration
        |
        +----------------+
        |                |
        v                v
    AI Provider     Observability
        |                |
        v                v
    AI Response     Execution Records
                         |
             +-----------+-----------+
             |           |           |
             v           v           v
          Metrics      Errors      Cost Data
             |           |           |
             +-----------+-----------+
                         |
                         v
                  Admin Monitoring

Observability must not expose sensitive candidate information.

# 4. AI Execution Tracking

Every AI execution must have a unique execution identifier.

The system should record:

| Field | Description |
|---|---|
| execution_id | Unique execution identifier |
| organization_id | Organization that initiated the operation |
| operation_type | Resume parsing, job analysis, matching, etc. |
| provider | Selected AI provider |
| model | Model identifier, if available |
| prompt_version | Prompt version |
| schema_version | Output schema version |
| status | Execution status |
| started_at | Execution start timestamp |
| completed_at | Execution completion timestamp |
| latency_ms | Total execution duration |
| input_tokens | Input tokens, if available |
| output_tokens | Output tokens, if available |
| estimated_cost | Estimated provider cost, if available |
| error_code | Normalized error code, if applicable |

Sensitive prompt content and complete resume text must not be stored in ordinary operational logs.

# 5. Execution Status

Supported execution states:

- PENDING
- PROCESSING
- COMPLETED
- FAILED
- TIMED_OUT
- CANCELLED

State transitions must be validated.

A completed execution must not silently become failed.

# 6. Performance Metrics

The system should monitor the following metrics:

| Metric | Purpose |
|---|---|
| Total AI executions | Overall usage |
| Successful executions | Reliability |
| Failed executions | Error monitoring |
| Success rate | Execution quality |
| Average latency | Performance |
| P95 latency | Tail performance |
| Timeout count | Provider reliability |
| Retry count | Retry behaviour |
| Provider availability | Integration health |

Metrics must be filterable by operation, provider and time period.

# 7. Token Usage Tracking

Where supported by the provider, record:

- Input tokens.
- Output tokens.
- Total tokens.
- Model identifier.
- Provider identifier.

If token usage is unavailable, record it as unknown.

Do not estimate token counts and represent them as actual provider-reported usage.

# 8. Cost Estimation

Cost estimates must use configured provider pricing information.

The calculation should consider:

- Provider.
- Model.
- Input token count.
- Output token count.
- Applicable pricing rates.
- Currency.
- Pricing configuration version.

Conceptual formula:

    Estimated Cost =
        (Input Tokens / 1,000,000 × Input Rate)
        +
        (Output Tokens / 1,000,000 × Output Rate)

The actual pricing unit must match the provider's published pricing.

Cost records must distinguish estimated costs from actual billed costs.

# 9. Cost Reporting

Future administrative reporting should include:

- Total estimated AI expenditure.
- Expenditure by provider.
- Expenditure by model.
- Expenditure by organization.
- Expenditure by operation.
- Average cost per execution.
- Cost trends over time.
- Token consumption trends.

During mock-provider development, monetary cost should remain zero unless a separate simulation is explicitly enabled.

# 10. Usage Limits

The architecture should support configurable limits.

Potential limits include:

- Maximum AI executions per organization.
- Maximum executions per day.
- Maximum executions per month.
- Maximum input size.
- Maximum output size.
- Maximum concurrent executions.
- Maximum estimated expenditure.

Initial MVP development should use conservative local configuration rather than implementing a paid subscription or billing system.

# 11. Alerts and Failure Detection

The system should identify:

- Repeated provider failures.
- Increased timeout rates.
- Unexpected latency increases.
- Unusually high execution volume.
- Abnormal token consumption.
- Cost threshold breaches.
- Invalid structured responses.

Alerts must be actionable and avoid exposing candidate personal information.

# 12. Logging Requirements

Structured logs should include:

- Timestamp.
- Log level.
- Request ID.
- Execution ID.
- Organization ID.
- Operation type.
- Provider.
- Execution status.
- Duration.
- Normalized error code.

Logs must exclude:

- API keys.
- Authentication tokens.
- Full resumes.
- Unnecessary candidate personal information.
- Sensitive provider credentials.

# 13. Data Retention

AI execution records must follow the approved retention policy.

Requirements:

1. Retention periods must be configurable.
2. Access must be restricted.
3. Deletion requests must be handled according to applicable privacy requirements.
4. Operational logs must not become an unintended repository of candidate information.
5. Historical metrics should use aggregated or appropriately de-identified data where possible.

# 14. Development and Testing

During local development:

- Use the mock AI provider.
- Generate deterministic execution records.
- Test successful executions.
- Test failed executions.
- Test timeout handling.
- Test missing token metadata.
- Test cost calculations using sample pricing.
- Test usage-limit enforcement.
- Test log redaction.

Mock execution data must be clearly distinguished from real provider usage.

# 15. MVP Implementation Priorities

| Feature | MVP Priority |
|---|---|
| Execution identifiers | P0 |
| Execution status tracking | P0 |
| Structured error logging | P0 |
| Basic latency measurement | P0 |
| Provider and model metadata | P0 |
| Token tracking when available | P1 |
| Cost estimation | P1 |
| Usage dashboards | P2 |
| Automated alerts | P2 |
| Advanced cost optimization | P2 |

# 16. Future Enhancements

Potential future capabilities:

- OpenTelemetry integration.
- Prometheus metrics.
- Grafana dashboards.
- Centralized log aggregation.
- Automated cost anomaly detection.
- Provider-level performance comparisons.
- Budget notifications.
- Cost-aware provider routing.
- Organization-specific AI usage budgets.

These capabilities should be introduced when operational complexity justifies them.

# 17. Acceptance Criteria

The implementation will satisfy this specification when:

1. Every AI execution has a unique identifier.
2. Execution status is recorded correctly.
3. Provider failures are normalized.
4. Execution latency is measurable.
5. Available token usage is recorded.
6. Cost calculations use versioned pricing configuration.
7. Sensitive information is excluded from operational logs.
8. Mock execution data is distinguishable from real provider data.
9. Usage limits can be configured and tested.
10. Monitoring does not interfere with core recruitment workflows.

# 18. Document Governance

This document must remain consistent with:

- AI_ARCHITECTURE.md
- AI_PROMPT_AND_SCHEMA_SPECIFICATION.md
- AI_EVALUATION_AND_TESTING.md
- AI_PROVIDER_INTEGRATION_SPECIFICATION.md
- API_STANDARDS.md
- DATABASE_DESIGN.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md

Changes to execution tracking, retention, pricing calculations or usage controls must be reviewed before implementation.

---

**Document Status:** Draft — Pending Review

**End of Document**