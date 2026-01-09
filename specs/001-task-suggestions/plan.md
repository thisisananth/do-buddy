# Implementation Plan: Smart Task Suggestions

**Branch**: `001-task-suggestions` | **Date**: 2026-01-08 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-task-suggestions/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

Build a web application that helps users productively use small blocks of free time by suggesting tasks from their todo lists. The system collects user context (available time, energy level, optional emotional state), uses AI to match tasks from imported todo lists, and breaks selected tasks into actionable steps. MVP uses text file import (one task per line) with AI-powered matching and task breakdown. Future versions will integrate with popular todo services.

## Technical Context

**Language/Version**: Python 3.11+ (backend), TypeScript/React 18+ (frontend)  
**Primary Dependencies**: FastAPI, React, OpenAI SDK, Pydantic, SQLite  
**Storage**: SQLite (backend), browser localStorage (frontend session)  
**Testing**: pytest (backend), Vitest (frontend), Playwright (E2E)  
**Target Platform**: Web application (modern browsers)  
**Project Type**: web (frontend + backend)  
**Performance Goals**: Task suggestion within 30 seconds (SC-001), alternative suggestions within 5 seconds (SC-005), AI API response time < 10 seconds  
**Constraints**: AI API rate limits and costs, browser storage limits for todo lists, no authentication for MVP (session-based)  
**Scale/Scope**: MVP - single user sessions, no persistent user accounts, todo lists stored in browser localStorage

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### I. API-First Design ✅
- **Status**: PASS
- **Compliance**: REST API contracts will be defined first in Phase 1. Frontend and backend communicate via well-defined API endpoints.
- **Action**: Generate OpenAPI contracts in `/contracts/` directory

### II. Test-First Development (NON-NEGOTIABLE) ✅
- **Status**: PASS
- **Compliance**: TDD will be enforced. Tests written before implementation. Target 80%+ coverage on critical paths.
- **Action**: Include test tasks in task breakdown, ensure test infrastructure setup

### III. Security & Authentication ⚠️
- **Status**: PARTIAL - MVP exception justified
- **Compliance**: MVP uses session-based storage (no auth). HTTPS required for production. Input validation required.
- **Justification**: MVP focuses on core functionality. Authentication deferred to future version per Simplicity principle.
- **Action**: Implement input validation, plan for future OAuth 2.0/JWT integration

### IV. Observability & Logging ✅
- **Status**: PASS
- **Compliance**: Structured logging with request IDs required. Error logging with context.
- **Action**: Include logging infrastructure in setup tasks

### V. Versioning & Breaking Changes ✅
- **Status**: PASS
- **Compliance**: API versioning strategy will be defined. Semantic versioning for API.
- **Action**: Define API versioning scheme (e.g., /api/v1/)

### VI. Simplicity & YAGNI ✅
- **Status**: PASS
- **Compliance**: MVP approach (text import, no auth) aligns with simplicity. Future enhancements deferred.
- **Action**: Keep implementation simple, avoid over-engineering

## Project Structure

### Documentation (this feature)

```text
specs/001-task-suggestions/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
backend/
├── src/
│   ├── models/          # Task, UserContext, TaskStep models
│   ├── services/        # AI matching service, task breakdown service, quick win generator
│   └── api/            # REST endpoints (context, suggestions, task breakdown)
└── tests/
    ├── contract/       # API contract tests
    ├── integration/    # Integration tests
    └── unit/          # Unit tests

frontend/
├── src/
│   ├── components/     # Time/Energy/Emotion inputs, Task display, Step display
│   ├── pages/          # Main app page, import page
│   └── services/       # API client
└── tests/
    ├── integration/    # Component integration tests
    └── unit/          # Component unit tests
```

**Structure Decision**: Web application structure selected (frontend + backend) to enable independent development and testing per API-First principle. Backend handles AI integration and business logic. Frontend handles user interactions and displays.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

No violations requiring justification. MVP approach aligns with Simplicity principle.

## Phase 0: Research Complete ✅

All technical context unknowns resolved. See `research.md` for detailed decisions:
- Backend: Python 3.11+ with FastAPI
- Frontend: React 18+ with TypeScript
- AI: OpenAI GPT-3.5-turbo/GPT-4
- Storage: SQLite (backend), localStorage (frontend)
- Testing: pytest, Vitest, Playwright

## Phase 1: Design Complete ✅

**Data Model**: See `data-model.md` for entity definitions, relationships, and validation rules.

**API Contracts**: See `contracts/openapi.yaml` for complete REST API specification with:
- POST /api/v1/context - Set user context
- POST /api/v1/tasks/import - Import todo list
- POST /api/v1/suggestions - Get task suggestion
- POST /api/v1/suggestions/alternative - Get alternative suggestion
- POST /api/v1/tasks/{task_id}/breakdown - Break task into steps

**Quickstart Guide**: See `quickstart.md` for setup instructions and development workflow.

### Constitution Check Post-Design ✅

All gates pass:
- ✅ API-First: OpenAPI contracts defined
- ✅ Test-First: Test infrastructure planned
- ✅ Security: Input validation defined in data model
- ✅ Observability: Logging requirements documented
- ✅ Versioning: API versioning (/api/v1/) implemented
- ✅ Simplicity: MVP approach maintained

## Next Steps

Ready for `/speckit.tasks` to generate implementation tasks.
