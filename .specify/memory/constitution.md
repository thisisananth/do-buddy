<!--
Sync Impact Report:
Version change: N/A → 1.0.0 (initial constitution)
Modified principles: N/A (all new)
Added sections: Technology Stack & Standards, Development Workflow
Removed sections: N/A
Templates requiring updates:
  ✅ .specify/templates/plan-template.md (Constitution Check section aligns with principles)
  ✅ .specify/templates/spec-template.md (scope/requirements align with principles)
  ✅ .specify/templates/tasks-template.md (task categorization reflects principles)
  ⚠ .cursor/commands/*.md (command files may reference constitution - manual review recommended)
Follow-up TODOs: None
-->

# Do Buddy Constitution

## Core Principles

### I. API-First Design

All features MUST be designed with API contracts defined first. Backend APIs MUST be
self-contained, independently testable, and documented. API endpoints MUST follow RESTful
conventions with clear request/response schemas. Frontend and backend MUST communicate
exclusively through well-defined API contracts. Clear purpose required - no endpoints
that exist solely for organizational convenience.

**Rationale**: API-first design ensures clear boundaries between frontend and backend,
enables independent development and testing, and supports future extensibility.

### II. Test-First Development (NON-NEGOTIABLE)

TDD mandatory: Tests written → User approved → Tests fail → Then implement.
Red-Green-Refactor cycle strictly enforced. All new features MUST have tests written
before implementation begins. Tests MUST cover happy paths, error cases, and edge
conditions. Test coverage MUST be maintained above 80% for critical paths.

**Rationale**: Test-first development catches defects early, provides living
documentation, and enables confident refactoring. This principle is non-negotiable
because it directly impacts code quality and maintainability.

### III. Security & Authentication

All API endpoints MUST implement appropriate authentication and authorization. User
data MUST be protected in transit (HTTPS) and at rest (encryption). Authentication
MUST use industry-standard protocols (OAuth 2.0, JWT, or equivalent). Authorization
MUST be enforced at the API layer with role-based access control (RBAC) where
applicable. Input validation and sanitization MUST be applied to all user inputs.
Security vulnerabilities MUST be addressed immediately upon discovery.

**Rationale**: Security is foundational for web applications handling user data.
Proactive security measures prevent breaches and protect user trust.

### IV. Observability & Logging

Structured logging MUST be implemented across all services. Logs MUST include
request IDs for traceability. Critical operations MUST emit metrics for monitoring.
Error conditions MUST be logged with sufficient context for debugging. Log levels
MUST be appropriately configured (DEBUG, INFO, WARN, ERROR). Production logs MUST
not contain sensitive information (passwords, tokens, PII).

**Rationale**: Observability enables rapid debugging, performance monitoring, and
operational insights. Structured logging ensures consistent parsing and analysis.

### V. Versioning & Breaking Changes

API versioning MUST follow semantic versioning (MAJOR.MINOR.PATCH). Breaking changes
MUST increment MAJOR version. Non-breaking additions increment MINOR version. Bug fixes
increment PATCH version. Breaking changes MUST include migration guides and deprecation
notices. Backward compatibility MUST be maintained for at least one MAJOR version cycle
when possible.

**Rationale**: Versioning provides clear communication about API changes and enables
clients to adapt gradually. Breaking changes require careful planning and communication.

### VI. Simplicity & YAGNI

Start simple, add complexity only when justified. YAGNI (You Aren't Gonna Need It)
principles MUST be followed. Over-engineering MUST be avoided. Complexity MUST be
justified in code reviews. Prefer standard solutions over custom implementations.
Architectural decisions MUST balance current needs with reasonable future flexibility.

**Rationale**: Simplicity reduces maintenance burden, improves developer velocity, and
decreases bug surface area. Premature optimization and over-engineering lead to
technical debt.

## Technology Stack & Standards

**Backend**: Technology stack MUST be selected based on project requirements. APIs MUST
follow RESTful conventions or GraphQL standards as appropriate. Database choices MUST
consider data consistency, scalability, and query patterns.

**Frontend**: Frontend framework MUST be selected based on project requirements. UI
components MUST be reusable and maintainable. State management MUST be clearly defined
and consistent across the application.

**Testing**: Unit tests MUST use appropriate testing frameworks. Integration tests
MUST cover API contracts and critical user journeys. End-to-end tests MUST validate
complete user workflows. Test infrastructure MUST be maintainable and fast.

**Deployment**: Applications MUST be containerized for consistent deployment.
Environment configuration MUST be externalized. Secrets MUST be managed securely
(not committed to version control). CI/CD pipelines MUST include automated testing
and deployment gates.

## Development Workflow

**Code Review**: All code changes MUST be reviewed before merge. Reviews MUST verify
constitution compliance, test coverage, and code quality. At least one approval
required for merge. Reviewers MUST check for security vulnerabilities, performance
issues, and adherence to established patterns.

**Branching Strategy**: Feature branches MUST be created from main/master. Branch
names MUST follow convention (e.g., `###-feature-name`). Branches MUST be kept
up-to-date with base branch. Merge commits MUST include clear descriptions.

**Testing Gates**: All tests MUST pass before merge. Test coverage MUST not decrease.
Integration tests MUST pass in CI environment. Performance regressions MUST be
identified and addressed.

**Documentation**: API endpoints MUST be documented (OpenAPI/Swagger or equivalent).
Complex logic MUST include inline comments. README files MUST be kept current.
Architecture decisions MUST be documented in ADRs (Architecture Decision Records)
when significant.

## Governance

This constitution supersedes all other development practices and guidelines.
Amendments to this constitution require:

1. **Documentation**: Proposed changes MUST be documented with rationale
2. **Review**: Changes MUST be reviewed and approved by project maintainers
3. **Versioning**: Constitution version MUST be incremented per semantic versioning
4. **Propagation**: Dependent templates and documentation MUST be updated to reflect
   changes

All pull requests and code reviews MUST verify compliance with this constitution.
Complexity and deviations MUST be justified. Constitution violations MUST be addressed
before merge.

**Compliance Review**: Regular reviews (quarterly recommended) MUST assess adherence
to principles and identify areas for improvement. Constitution updates MUST be
communicated to all team members.

**Version**: 1.0.0 | **Ratified**: 2026-01-06 | **Last Amended**: 2026-01-06
