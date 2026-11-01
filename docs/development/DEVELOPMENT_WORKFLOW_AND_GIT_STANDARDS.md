# Niyuvia — Development Workflow and Git Standards

**Product:** Niyuvia  
**Document:** Development Workflow and Git Standards  
**Version:** 1.0  
**Status:** Draft — Pending Review  
**Last Updated:** 1 November 2026  
**Owner:** Engineering

---

# 1. Purpose

This document defines the standard development workflow, Git conventions, code review practices and repository management procedures for Niyuvia.

The objectives are to:

- Maintain a clean and understandable codebase.
- Establish consistent development practices.
- Make changes traceable.
- Reduce integration problems.
- Support future collaboration.
- Maintain reliable version history.

# 2. Repository Structure

Niyuvia uses a monorepo structure.

The repository contains:

- Frontend application.
- Backend application.
- Product documentation.
- Architecture documentation.
- Database documentation.
- API documentation.
- AI documentation.
- Infrastructure configuration.
- Development scripts.

Each application must maintain clear module boundaries and responsibilities.

# 3. Git Branching Strategy

The repository uses the following branch conventions:

| Branch | Purpose |
|---|---|
| main | Stable, reviewed code |
| develop | Integration branch, when required |
| feature/* | New functionality |
| fix/* | Bug fixes |
| docs/* | Documentation changes |
| refactor/* | Internal code improvements |
| test/* | Testing-related changes |

For the initial solo development phase, working directly on main is acceptable for small, controlled changes.

Feature branches should be introduced as development complexity increases.

# 4. Branch Naming

Branch names must be descriptive and use lowercase words separated by hyphens.

Examples:

- feature/recruiter-authentication
- feature/job-management
- feature/resume-upload
- feature/ai-resume-parsing
- fix/candidate-validation
- docs/api-standards
- refactor/candidate-service
- test/ai-matching

# 5. Commit Message Convention

Commits should follow Conventional Commits.

Format:

    <type>(optional-scope): <description>

Supported types:

| Type | Purpose |
|---|---|
| feat | New functionality |
| fix | Bug correction |
| docs | Documentation changes |
| refactor | Code restructuring |
| test | Test additions or changes |
| chore | Maintenance |
| build | Build configuration |
| ci | CI/CD changes |
| perf | Performance improvements |
| security | Security-related changes |

Examples:

    feat(auth): add recruiter authentication

    feat(jobs): implement job creation API

    fix(candidates): validate candidate email

    docs(architecture): document backend boundaries

    test(ai): add resume parser validation tests

Commit messages must describe the actual change.

# 6. Development Workflow

Every feature should follow this sequence:

1. Review the relevant product requirements.
2. Identify affected modules.
3. Review architecture and API contracts.
4. Create or select an appropriate Git branch.
5. Implement the smallest coherent change.
6. Add or update tests.
7. Run relevant quality checks.
8. Review the complete changeset.
9. Commit the changes.
10. Merge only after validation.

# 7. Documentation-First Development

Before implementing a major feature:

- Review the relevant PRD requirements.
- Review functional requirements.
- Review associated user stories.
- Review business rules.
- Review database design.
- Review API specifications.
- Review AI specifications when applicable.

Implementation must not silently contradict approved product documentation.

If requirements change, update the affected documentation and review related dependencies.

# 8. Code Quality Standards

All code must follow these principles:

1. Clear naming conventions.
2. Small, focused functions.
3. Separation of concerns.
4. Reusable components where justified.
5. Explicit error handling.
6. Input validation.
7. Type safety.
8. Consistent formatting.
9. Minimal unnecessary dependencies.
10. Avoidance of duplicated business logic.

Code must be understandable to another engineer without requiring undocumented explanations.

# 9. Frontend Standards

Frontend development must follow:

- TypeScript for application code.
- Reusable UI components.
- Clear feature-based organization.
- Consistent styling conventions.
- Centralized API communication.
- Explicit loading and error states.
- Form validation.
- Accessible interaction patterns.
- Responsive layouts.

Business logic should not be unnecessarily embedded in presentation components.

# 10. Backend Standards

Backend development must follow:

- Clear separation of API and service layers.
- Pydantic request and response validation.
- Explicit database access patterns.
- Consistent exception handling.
- Authentication and authorization enforcement.
- Structured logging.
- Transaction management.
- Versioned API contracts.
- Testable business logic.

Database queries and business rules should not be unnecessarily mixed into route handlers.

# 11. Database Change Management

Database schema changes must use version-controlled migrations.

Requirements:

1. Never rely on undocumented manual schema changes.
2. Every schema change must have a migration.
3. Review migration dependencies.
4. Test migrations against a local database.
5. Consider rollback or recovery procedures.
6. Avoid destructive changes without a documented migration plan.
7. Keep database documentation synchronized.

# 12. Environment and Configuration Management

Environment-specific settings must be managed through configuration.

Requirements:

- Keep secrets outside source code.
- Maintain `.env.example`.
- Never commit `.env` files containing credentials.
- Validate required environment variables.
- Separate local, test, staging and production configuration.
- Document configuration changes.

# 13. Pull Request Standards

When collaboration begins, pull requests should include:

- Clear title.
- Summary of changes.
- Reason for the change.
- Related requirement or issue.
- Testing performed.
- Screenshots when UI changes are involved.
- Migration notes where applicable.
- Known limitations.

Pull requests should remain focused and reasonably sized.

# 14. Code Review Checklist

Reviewers should verify:

- Requirements are satisfied.
- Architecture boundaries are respected.
- No unnecessary dependencies were introduced.
- Input validation is implemented.
- Authorization is enforced.
- Sensitive information is protected.
- Error handling is appropriate.
- Tests cover important behaviour.
- Documentation is updated.
- No secrets or unrelated generated files are committed.

# 15. Git Safety Rules

The following practices are mandatory:

- Do not commit credentials.
- Do not force-push shared branches without agreement.
- Do not rewrite stable shared history without approval.
- Do not commit build artifacts unless explicitly required.
- Do not commit temporary files.
- Review `git status` before committing.
- Review staged changes before pushing.
- Keep `.gitignore` updated.

# 16. Local Development Workflow

The standard local workflow is:

    git status
    git pull
    git checkout -b feature/feature-name

After implementation:

    git status
    git diff
    git add .
    git diff --cached
    git commit -m "feat(scope): describe change"

Before pushing:

    git push -u origin feature/feature-name

These commands must be adapted to the current branch strategy and repository state.

Do not run destructive Git commands without first reviewing their impact.

# 17. Quality Validation Before Commit

Before committing a change, run the relevant checks:

- Frontend linting.
- Frontend type checking.
- Backend linting.
- Backend unit tests.
- Integration tests.
- Production build.
- Database migration validation.

Not every check needs to run for every documentation-only change, but all relevant checks must pass.

# 18. Dependency Management

Dependencies must be introduced deliberately.

Requirements:

1. Prefer maintained packages.
2. Review licensing and security implications.
3. Avoid unnecessary overlapping libraries.
4. Keep lockfiles committed.
5. Review dependency updates.
6. Remove unused dependencies.
7. Record important architectural dependency decisions.

# 19. Release Management

A release must:

- Have a reviewed changeset.
- Pass mandatory quality checks.
- Include relevant database migrations.
- Have documented configuration requirements.
- Have known limitations recorded.
- Follow the release quality gates.

Production deployment must remain separate from ordinary development commits.

# 20. Acceptance Criteria

This workflow is considered established when:

1. Git conventions are documented.
2. Repository structure is understood.
3. Commit messages are consistent.
4. Environment secrets are protected.
5. Database migrations are version-controlled.
6. Testing is part of feature development.
7. Changes are traceable.
8. Documentation changes are reviewed.
9. Release checks are defined.
10. Another developer can understand the workflow.

# 21. Document Governance

This document must remain consistent with:

- PRD.md
- PRODUCT_VISION.md
- ARCHITECTURE.md
- DATABASE_DESIGN.md
- API_STANDARDS.md
- TEST_STRATEGY_AND_QUALITY_PLAN.md
- SECURITY_AND_PRIVACY_REQUIREMENTS.md
- NON_FUNCTIONAL_REQUIREMENTS.md

Changes to development standards must be reviewed when the repository architecture or team workflow changes.

---

**Document Status:** Draft — Pending Review