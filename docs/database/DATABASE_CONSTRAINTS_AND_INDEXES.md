# Niyuvia — Database Constraints and Indexing Specification

**Product:** Niyuvia  
**Document:** Database Constraints and Indexing Specification  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Database:** PostgreSQL  
**ORM:** SQLAlchemy 2.x  
**Last Updated:** 1 November 2026

---

# 1. Purpose

This document defines database-level integrity constraints, indexing strategies, relationship rules and schema enforcement requirements for Niyuvia.

The objective is to maintain:

- Data integrity.
- Referential consistency.
- Organization-level data isolation.
- Reliable recruitment workflows.
- Efficient database queries.
- Safe schema evolution.

This document supplements:

- DATABASE_DESIGN.md
- DATABASE_DATA_DICTIONARY.md
- ARCHITECTURE.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- BUSINESS_RULES.md

# 2. Database Standards

| Standard | Requirement |
|---|---|
| Database engine | PostgreSQL |
| Primary identifiers | UUID |
| Timestamp storage | TIMESTAMPTZ |
| Default timezone | UTC |
| Structured data | JSONB |
| ORM | SQLAlchemy 2.x |
| Migration tool | Alembic |
| Referential integrity | Foreign keys |
| Schema versioning | Alembic migrations |

All production database changes must be migration-controlled.

# 3. Primary Key Constraints

Every primary entity must have a unique, immutable primary identifier.

Primary keys should use UUID values.

Expected entities include:

- organizations
- users
- organization_members
- departments
- jobs
- job_requirements
- candidates
- applications
- resumes
- resume_extractions
- ai_runs
- ai_evaluations
- evaluation_evidence
- application_events
- interviews
- interview_feedback
- audit_logs
- privacy_records

Requirements:

1. Every entity must have exactly one primary key.
2. Primary keys must not be nullable.
3. Primary keys must not change after creation.
4. Related records must reference valid parent records.

# 4. Foreign Key Constraints

Foreign keys must maintain valid relationships between database entities.

| Child Table | Foreign Key | Parent Table |
|---|---|---|
| organization_members | organization_id | organizations |
| organization_members | user_id | users |
| departments | organization_id | organizations |
| jobs | organization_id | organizations |
| jobs | department_id | departments |
| job_requirements | job_id | jobs |
| candidates | organization_id | organizations |
| applications | job_id | jobs |
| applications | candidate_id | candidates |
| resumes | candidate_id | candidates |
| resume_extractions | resume_id | resumes |
| ai_evaluations | application_id | applications |
| ai_evaluations | ai_run_id | ai_runs |
| evaluation_evidence | evaluation_id | ai_evaluations |
| application_events | application_id | applications |
| interviews | application_id | applications |
| interview_feedback | interview_id | interviews |

## 4.1 Organization Ownership

All organization-owned records must maintain valid organization ownership.

Where appropriate, use composite foreign keys to ensure that related records belong to the same organization.

Example:

An application must not connect:

- A candidate belonging to Organization A.
- A job belonging to Organization B.

This must be prevented at the database level, not only through frontend validation.

## 4.2 Deletion Policies

| Relationship | Recommended Behavior |
|---|---|
| Organization to jobs | RESTRICT |
| Organization to candidates | RESTRICT |
| Organization to applications | RESTRICT |
| Candidate to applications | RESTRICT |
| Job to applications | RESTRICT |
| Application to application_events | RESTRICT |
| Resume to extraction records | RESTRICT |
| Interview to feedback | RESTRICT |

Deletion must follow an approved retention and privacy policy.

Do not use unrestricted cascading deletion for recruitment history, audit records or privacy records.

# 5. Unique Constraints

The following uniqueness requirements should be enforced.

| Table | Field or Combination | Requirement |
|---|---|---|
| organizations | slug | Globally unique |
| users | email | Globally unique after normalization |
| organization_members | organization_id + user_id | Unique membership |
| departments | organization_id + name | Organization-scoped uniqueness |
| job_requirements | job_id + requirement_name + version | Version-aware uniqueness |

Candidate email addresses must not be globally unique because candidates may exist in different organizations.

Candidate deduplication must be organization-scoped and must account for legitimate shared contact information.

# 6. Check Constraints

The database must enforce important data validity rules.

## 6.1 Job Constraints

- experience_min must be zero or greater.
- experience_max must be zero or greater.
- experience_max must not be lower than experience_min.
- status must belong to the approved job status values.

## 6.2 Resume Constraints

- file_size_bytes must be greater than zero.
- checksum_sha256 must contain a valid SHA-256 hexadecimal value when supplied.
- processing_status must belong to the approved status values.

## 6.3 AI Evaluation Constraints

- alignment_score must be between 0 and 100 when supplied.
- evidence_coverage must be between 0 and 100 when supplied.
- scoring_version must not be empty.

## 6.4 Interview Constraints

- duration_minutes must be positive when supplied.
- rating must remain within the configured rating scale.

## 6.5 General Constraints

- Required fields must be NOT NULL.
- Numeric values must respect their documented ranges.
- Status fields must use controlled values.
- Invalid records must fail database validation.

# 7. Indexing Strategy

Indexes must support frequently executed queries without creating unnecessary write overhead.

## 7.1 Organization Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| organizations | slug | Organization lookup |
| organization_members | organization_id, user_id | Membership lookup |
| organization_members | user_id | User organization lookup |

## 7.2 Job Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| jobs | organization_id | Organization job listing |
| jobs | organization_id, status | Job status filtering |
| jobs | organization_id, created_at | Recent job listing |
| jobs | organization_id, department_id | Department filtering |
| job_requirements | job_id | Requirement retrieval |

## 7.3 Candidate Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| candidates | organization_id | Organization candidate listing |
| candidates | organization_id, created_at | Recent candidate listing |
| candidates | organization_id, email | Candidate lookup |
| candidates | organization_id, full_name | Candidate search |

Candidate name search may require PostgreSQL trigram indexes in a later optimization phase.

## 7.4 Application Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| applications | organization_id | Organization application listing |
| applications | organization_id, job_id | Job-specific applications |
| applications | organization_id, candidate_id | Candidate application history |
| applications | organization_id, stage | Recruitment pipeline filtering |
| applications | organization_id, created_at | Recent applications |
| application_events | application_id, created_at | Application history |

## 7.5 Resume Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| resumes | organization_id, candidate_id | Candidate resume retrieval |
| resumes | organization_id, processing_status | Processing queue filtering |
| resume_extractions | resume_id | Extraction lookup |

## 7.6 AI Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| ai_runs | organization_id, created_at | AI execution history |
| ai_runs | status, created_at | Processing monitoring |
| ai_evaluations | organization_id, application_id | Evaluation lookup |
| ai_evaluations | application_id, created_at | Evaluation history |
| evaluation_evidence | evaluation_id | Evidence retrieval |

## 7.7 Interview Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| interviews | organization_id, scheduled_at | Interview calendar |
| interviews | application_id | Application interview history |
| interview_feedback | interview_id | Feedback retrieval |

## 7.8 Audit and Privacy Indexes

| Table | Index Fields | Purpose |
|---|---|---|
| audit_logs | organization_id, created_at | Organization audit history |
| audit_logs | actor_user_id, created_at | User activity |
| privacy_records | organization_id, candidate_id | Candidate privacy records |
| privacy_records | status, recorded_at | Pending privacy requests |

# 8. Index Design Rules

1. Avoid creating indexes on every column.
2. Prioritize frequently used filters and joins.
3. Use composite indexes according to actual query patterns.
4. Consider column ordering carefully.
5. Avoid duplicate indexes.
6. Monitor index size and query performance.
7. Use EXPLAIN ANALYZE to evaluate expensive queries.
8. Introduce specialized indexes only when justified by measured workloads.

Indexes must not be added solely because a column exists.

# 9. Multi-Tenant Data Isolation

Niyuvia is designed for organization-scoped data access.

The database and backend must enforce:

- Organization ownership.
- Tenant-aware foreign key relationships.
- Organization-scoped queries.
- Authorized cross-organization access restrictions.
- Role-based access control.

For commercial production, PostgreSQL Row-Level Security (RLS) should be evaluated and implemented where appropriate.

RLS must not be considered implemented merely because it is documented.

Tenant context must be established securely by the backend.

# 10. Soft Deletion and Retention

Soft deletion may be used for selected records where historical traceability is required.

Potential fields:

- deleted_at
- deleted_by

However:

- Soft deletion is not a substitute for data erasure obligations.
- Retention periods must be defined.
- Expired data must be handled according to approved retention policies.
- Legal holds and audit requirements must be considered.
- Backups must follow documented retention and deletion procedures.

# 11. Migration Management

Alembic must manage all database schema changes.

Every migration must:

1. Have a unique revision identifier.
2. Include a meaningful description.
3. Define upgrade operations.
4. Define downgrade operations where safely possible.
5. Preserve existing data unless migration explicitly requires transformation.
6. Be tested against a representative database.
7. Be reviewed before production execution.

Never manually modify a production schema without a controlled migration.

# 12. Query Performance Requirements

Database operations should:

- Use pagination for large result sets.
- Avoid unnecessary SELECT * queries.
- Avoid N+1 query patterns.
- Use appropriate joins.
- Restrict queries by organization.
- Avoid loading entire resume files into database query results.
- Use connection pooling.
- Apply query timeouts where appropriate.

Performance targets must be validated against a defined workload before being treated as service-level commitments.

# 13. Database Security Requirements

- Use parameterized queries.
- Use least-privilege database accounts.
- Keep database credentials outside source control.
- Restrict network access to the database.
- Encrypt database connections in production.
- Protect backups.
- Restrict access to candidate personal data.
- Never expose internal database errors to end users.

# 14. Testing Requirements

Database tests must cover:

- Primary key uniqueness.
- Foreign key integrity.
- Unique constraints.
- Check constraints.
- Organization ownership validation.
- Invalid status rejection.
- Migration upgrade and downgrade behavior.
- Concurrent update scenarios.
- Index creation.
- Tenant isolation.

Tests must use a dedicated test database.

# 15. Future Optimization

The following optimizations are deferred until justified by actual workload:

- PostgreSQL full-text search.
- Trigram indexes.
- Partial indexes.
- Covering indexes.
- Partitioning of high-volume audit tables.
- pgvector indexes.
- Materialized analytics views.
- Advanced query optimization.

Do not introduce advanced database infrastructure before it is needed.

# 16. Document Governance

This specification must remain synchronized with:

- DATABASE_DESIGN.md
- DATABASE_DATA_DICTIONARY.md
- ARCHITECTURE.md
- BUSINESS_RULES.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md

Any schema or relationship change must trigger a review of this document.

---

**Document Status:** Draft — Pending Review

**End of Document**