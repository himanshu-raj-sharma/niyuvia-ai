# Niyuvia — Release and Change Management

**Product:** Niyuvia  
**Document:** Release and Change Management Plan  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Owner:** Engineering

---

# 1. Purpose

This document defines the processes for managing software changes, preparing releases, validating release candidates and maintaining application stability.

The objectives are to:

- Establish a predictable release process.
- Maintain traceable software changes.
- Prevent unverified changes from reaching production.
- Reduce deployment risks.
- Define release approval requirements.
- Establish rollback and recovery procedures.

# 2. Scope

This document covers:

- Feature changes.
- Bug fixes.
- Documentation changes.
- Database migrations.
- Dependency updates.
- Configuration changes.
- Release preparation.
- Deployment validation.
- Rollback procedures.
- Post-release monitoring.

# 3. Release Environments

Niyuvia should support the following environments.

| Environment | Purpose |
|---|---|
| Local | Developer implementation |
| Test | Automated and integration testing |
| Staging | Release candidate validation |
| Production | Live commercial application |

During the zero-budget MVP phase, local and test environments are the primary development environments.

Staging and production infrastructure will be introduced when justified.

# 4. Release Types

| Release Type | Description |
|---|---|
| Major | Significant architectural or product changes |
| Minor | New backward-compatible functionality |
| Patch | Bug fixes and small improvements |
| Hotfix | Urgent correction of a production defect |
| Internal | Development-only milestone |

Release numbering should follow Semantic Versioning:

    MAJOR.MINOR.PATCH

Example:

    1.0.0
    1.1.0
    1.1.1

The versioning policy should be applied consistently across application releases.

# 5. Change Classification

Every proposed change should be classified.

| Classification | Description |
|---|---|
| Feature | New functionality |
| Bug Fix | Existing behaviour correction |
| Refactor | Internal restructuring |
| Security | Security improvement or vulnerability correction |
| Infrastructure | Deployment or environment change |
| Database | Schema or migration change |
| Documentation | Specification or technical documentation change |

# 6. Change Management Workflow

Every significant change should follow this process:

1. Identify the change requirement.
2. Review affected documentation.
3. Assess architectural impact.
4. Identify affected modules.
5. Estimate implementation risk.
6. Implement the change.
7. Update relevant tests.
8. Run required quality checks.
9. Review the changeset.
10. Approve and integrate the change.
11. Update release documentation where applicable.

Changes must not silently alter approved business rules.

# 7. Release Preparation

Before preparing a release candidate:

- Confirm the release scope.
- Review completed features.
- Review unresolved defects.
- Verify database migrations.
- Validate environment configuration.
- Run relevant automated tests.
- Verify frontend and backend builds.
- Review security-sensitive changes.
- Update release notes.

# 8. Release Quality Gates

A release candidate must satisfy:

1. Required functional tests pass.
2. Critical security checks pass.
3. No unresolved critical defects remain.
4. Database migrations are validated.
5. API contracts remain consistent.
6. Environment variables are documented.
7. Essential user workflows are verified.
8. Build and startup checks pass.
9. Known limitations are documented.
10. Release approval is recorded.

# 9. Database Migration Management

Database changes require particular care.

Requirements:

- Use version-controlled migrations.
- Review migration dependencies.
- Test migrations locally.
- Validate upgrade behaviour.
- Consider rollback and recovery.
- Back up production data before risky migrations.
- Avoid undocumented manual database changes.
- Document breaking schema changes.

A migration must not be considered safe merely because it succeeds on a local development database.

# 10. Configuration Management

Application configuration must be separated by environment.

Requirements:

- Keep secrets outside source control.
- Maintain `.env.example`.
- Validate required configuration.
- Document configuration changes.
- Avoid development defaults in production.
- Restrict production configuration access.
- Review changes to security-sensitive settings.

# 11. Deployment Process

The proposed deployment process is:

    Change Approved
          |
          v
    Release Candidate
          |
          v
    Automated Quality Checks
          |
          v
    Staging Validation
          |
          v
    Release Approval
          |
          v
    Production Deployment
          |
          v
    Post-Deployment Verification

Production deployment must use a reviewed and reproducible build.

# 12. Rollback and Recovery

A rollback plan must exist for production releases.

Potential rollback triggers:

- Critical application failure.
- Data corruption.
- Security vulnerability.
- Payment or integration failure.
- Unavailable core recruitment workflows.
- Severe performance degradation.

Recovery procedures should include:

1. Identify the failed release.
2. Assess the impact.
3. Stop further deployment.
4. Restore the previous application version where safe.
5. Evaluate database compatibility.
6. Restore data from backups if necessary.
7. Verify essential workflows.
8. Record the incident and corrective actions.

Database rollback must not be performed blindly when data loss or migration incompatibility is possible.

# 13. Release Notes

Every release should include:

- Release version.
- Release date.
- Summary.
- New features.
- Bug fixes.
- Security changes.
- Database changes.
- Configuration changes.
- Known issues.
- Upgrade instructions where required.

Release notes must accurately describe shipped functionality.

# 14. Post-Release Validation

After deployment, verify:

- Application startup.
- Authentication.
- Database connectivity.
- Core recruitment workflows.
- Resume processing.
- AI integration health.
- API availability.
- Error logs.
- Performance indicators.
- Security-sensitive operations.

A successful deployment command alone does not prove that the application is functioning correctly.

# 15. Emergency Hotfix Process

Urgent production defects require a controlled hotfix workflow.

1. Record the incident.
2. Identify the affected functionality.
3. Assess severity and impact.
4. Implement the smallest safe correction.
5. Run targeted tests.
6. Review the change.
7. Deploy the correction.
8. Verify recovery.
9. Document the incident.
10. Add regression tests.

Hotfixes must not bypass essential security checks.

# 16. Release Documentation and Traceability

Each release should be traceable to:

- Related requirements.
- Associated user stories.
- Git commits.
- Pull requests where applicable.
- Test reports.
- Database migrations.
- Deployment records.
- Known defects.

This traceability supports future maintenance and engineering audits.

# 17. MVP Release Priorities

| Capability | Priority |
|---|---|
| Versioned Git history | P0 |
| Reproducible local builds | P0 |
| Basic release notes | P0 |
| Automated quality checks | P0 |
| Database migration validation | P0 |
| Environment configuration documentation | P0 |
| Production rollback plan | P1 |
| Staging deployment | P1 |
| Deployment monitoring | P1 |
| Automated release pipeline | P2 |

# 18. Zero-Budget Development Policy

During initial MVP development:

- Prefer free local development tools.
- Use mock AI providers.
- Avoid paid infrastructure.
- Avoid unnecessary external services.
- Keep deployment optional until the MVP is ready.
- Use local databases and synthetic test data.
- Keep production-specific requirements documented without purchasing infrastructure.

A future commercial release may introduce paid hosting, managed databases, monitoring and external AI providers after validation and budget approval.

# 19. Acceptance Criteria

This document is considered implemented when:

1. Change classification is established.
2. Release versions are traceable.
3. Quality gates are documented.
4. Database migration practices are defined.
5. Deployment validation is defined.
6. Rollback and recovery procedures are documented.
7. Release notes follow a consistent format.
8. Post-release checks are defined.
9. Zero-budget development constraints are respected.
10. Production release responsibilities are clear.

# 20. Document Governance

This document must remain consistent with:

- PRD.md
- PRODUCT_VISION.md
- ARCHITECTURE.md
- DATABASE_DESIGN.md
- API_STANDARDS.md
- TEST_STRATEGY_AND_QUALITY_PLAN.md
- DEVELOPMENT_WORKFLOW_AND_GIT_STANDARDS.md
- SECURE_CODING_STANDARDS.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md

Changes to release procedures must be reviewed when deployment architecture, quality gates or production requirements change.

---

**Document Status:** Draft — Pending Review