# Tasks: Smart Task Suggestions

**Input**: Design documents from `/specs/001-task-suggestions/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Tests are REQUIRED per constitution (Test-First Development principle). All tests must be written before implementation.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Web app**: `backend/src/`, `frontend/src/`
- Paths follow plan.md structure: backend/ and frontend/ directories

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create backend project structure (backend/src/api/, backend/src/models/, backend/src/services/, backend/tests/)
- [ ] T002 Create frontend project structure (frontend/src/components/, frontend/src/pages/, frontend/src/services/, frontend/tests/)
- [ ] T003 Initialize Python backend project with FastAPI dependencies in backend/requirements.txt
- [ ] T004 Initialize React frontend project with TypeScript and Vite in frontend/package.json
- [ ] T005 [P] Configure Python linting (ruff) and formatting (black) in backend/
- [ ] T006 [P] Configure TypeScript linting (ESLint) and formatting (Prettier) in frontend/
- [ ] T007 [P] Setup pytest configuration in backend/pytest.ini
- [ ] T008 [P] Setup Vitest configuration in frontend/vitest.config.ts
- [ ] T009 [P] Setup Playwright for E2E testing in frontend/
- [ ] T010 Create .env.example files for backend and frontend with required environment variables

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T011 Setup FastAPI application with API versioning (/api/v1/) in backend/src/api/main.py
- [ ] T012 [P] Configure CORS middleware for frontend-backend communication in backend/src/api/main.py
- [ ] T013 [P] Setup structured logging with request IDs in backend/src/api/middleware/logging.py
- [ ] T014 [P] Configure error handling middleware in backend/src/api/middleware/errors.py
- [ ] T015 [P] Setup environment configuration management (python-dotenv) in backend/src/config.py
- [ ] T016 [P] Create API client service in frontend/src/services/api.ts
- [ ] T017 [P] Setup React Router for navigation in frontend/src/App.tsx
- [ ] T018 [P] Create localStorage utility for session data management in frontend/src/utils/storage.ts
- [ ] T019 Setup SQLite database schema (optional caching tables) in backend/src/db/schema.sql
- [ ] T020 Create database connection utility in backend/src/db/connection.py

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Get Task Suggestion and Start First Step (Priority: P1) 🎯 MVP

**Goal**: User opens website, imports todo list, provides context (time/energy/emotion), receives task suggestion, accepts it, and sees first step

**Independent Test**: Can be fully tested by having a user with an existing todo list open the app, answer the context questions, accept a suggested task, and see the first step displayed. This delivers immediate value by helping users start productive work during available time blocks.

### Tests for User Story 1 ⚠️

> **NOTE: Write these tests FIRST, ensure they FAIL before implementation**

- [ ] T021 [P] [US1] Contract test for POST /api/v1/tasks/import in backend/tests/contract/test_import_tasks.py
- [ ] T022 [P] [US1] Contract test for POST /api/v1/context in backend/tests/contract/test_context.py
- [ ] T023 [P] [US1] Contract test for POST /api/v1/suggestions in backend/tests/contract/test_suggestions.py
- [ ] T024 [P] [US1] Contract test for POST /api/v1/tasks/{task_id}/breakdown in backend/tests/contract/test_breakdown.py
- [ ] T025 [P] [US1] Integration test for task import flow in backend/tests/integration/test_import_flow.py
- [ ] T026 [P] [US1] Integration test for suggestion flow in backend/tests/integration/test_suggestion_flow.py
- [ ] T027 [P] [US1] Integration test for breakdown flow in backend/tests/integration/test_breakdown_flow.py
- [ ] T028 [P] [US1] Component test for TodoImportPage in frontend/tests/integration/TodoImportPage.test.tsx
- [ ] T029 [P] [US1] Component test for ContextForm in frontend/tests/integration/ContextForm.test.tsx
- [ ] T030 [P] [US1] Component test for TaskSuggestion in frontend/tests/integration/TaskSuggestion.test.tsx
- [ ] T031 [P] [US1] Component test for TaskStepDisplay in frontend/tests/integration/TaskStepDisplay.test.tsx
- [ ] T032 [US1] E2E test for complete User Story 1 flow in frontend/tests/e2e/user-story-1.spec.ts

### Implementation for User Story 1

- [ ] T033 [P] [US1] Create UserContext Pydantic model in backend/src/models/user_context.py
- [ ] T034 [P] [US1] Create Task Pydantic model in backend/src/models/task.py
- [ ] T035 [P] [US1] Create TaskStep Pydantic model in backend/src/models/task_step.py
- [ ] T036 [P] [US1] Create TodoList Pydantic model in backend/src/models/todo_list.py
- [ ] T037 [P] [US1] Create UserContext TypeScript type in frontend/src/types/userContext.ts
- [ ] T038 [P] [US1] Create Task TypeScript type in frontend/src/types/task.ts
- [ ] T039 [P] [US1] Create TaskStep TypeScript type in frontend/src/types/taskStep.ts
- [ ] T040 [P] [US1] Create TodoList TypeScript type in frontend/src/types/todoList.ts
- [ ] T041 [US1] Implement OpenAI client wrapper in backend/src/services/ai_client.py
- [ ] T042 [US1] Implement AI task matching service in backend/src/services/ai_matching_service.py (depends on T041)
- [ ] T043 [US1] Implement AI task breakdown service in backend/src/services/ai_breakdown_service.py (depends on T041)
- [ ] T044 [US1] Implement QuickWinTask generator in backend/src/services/quick_win_service.py
- [ ] T045 [US1] Implement POST /api/v1/tasks/import endpoint in backend/src/api/routes/tasks.py
- [ ] T046 [US1] Implement POST /api/v1/context endpoint in backend/src/api/routes/context.py
- [ ] T047 [US1] Implement POST /api/v1/suggestions endpoint in backend/src/api/routes/suggestions.py (depends on T042, T044)
- [ ] T048 [US1] Implement POST /api/v1/tasks/{task_id}/breakdown endpoint in backend/src/api/routes/breakdown.py (depends on T043)
- [ ] T049 [US1] Create TodoImportPage component in frontend/src/pages/TodoImportPage.tsx
- [ ] T050 [US1] Create ContextForm component with time/energy/emotion inputs in frontend/src/components/ContextForm.tsx
- [ ] T051 [US1] Create TaskSuggestion component to display suggested task in frontend/src/components/TaskSuggestion.tsx
- [ ] T052 [US1] Create TaskStepDisplay component to show first step in frontend/src/components/TaskStepDisplay.tsx
- [ ] T053 [US1] Create main App page that orchestrates User Story 1 flow in frontend/src/pages/AppPage.tsx
- [ ] T054 [US1] Add input validation and error handling for all API endpoints
- [ ] T055 [US1] Add structured logging for all API operations with request IDs

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - Request Alternative Task Suggestion (Priority: P2)

**Goal**: User receives a task suggestion, clicks "suggest another task", and receives a different appropriate suggestion

**Independent Test**: Can be fully tested by having a user receive a task suggestion, click "suggest another task", and receive a different appropriate suggestion. This delivers value by ensuring users find tasks that match their current motivation and context.

### Tests for User Story 2 ⚠️

> **NOTE: Write these tests FIRST, ensure they FAIL before implementation**

- [ ] T056 [P] [US2] Contract test for POST /api/v1/suggestions/alternative in backend/tests/contract/test_alternative_suggestions.py
- [ ] T057 [P] [US2] Integration test for alternative suggestion flow in backend/tests/integration/test_alternative_flow.py
- [ ] T058 [P] [US2] Component test for AlternativeSuggestionButton in frontend/tests/integration/AlternativeSuggestionButton.test.tsx
- [ ] T059 [US2] E2E test for User Story 2 flow in frontend/tests/e2e/user-story-2.spec.ts

### Implementation for User Story 2

- [ ] T060 [US2] Update AI matching service to track excluded task IDs in backend/src/services/ai_matching_service.py
- [ ] T061 [US2] Implement POST /api/v1/suggestions/alternative endpoint in backend/src/api/routes/suggestions.py
- [ ] T062 [US2] Create AlternativeSuggestionButton component in frontend/src/components/AlternativeSuggestionButton.tsx
- [ ] T063 [US2] Update TaskSuggestion component to include alternative suggestion button in frontend/src/components/TaskSuggestion.tsx
- [ ] T064 [US2] Update AppPage to handle alternative suggestion requests in frontend/src/pages/AppPage.tsx
- [ ] T065 [US2] Add logic to prevent suggesting same task repeatedly (track suggested_count) in backend/src/services/ai_matching_service.py

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - Skip Optional Emotional State Question (Priority: P3)

**Goal**: User can skip the optional emotional state question and still receive appropriate task suggestions

**Independent Test**: Can be fully tested by having a user open the app, answer time and energy questions, skip the emotional state question, and still receive appropriate task suggestions. This delivers value by maintaining functionality while respecting user privacy preferences.

### Tests for User Story 3 ⚠️

> **NOTE: Write these tests FIRST, ensure they FAIL before implementation**

- [ ] T066 [P] [US3] Integration test for suggestion flow without emotional state in backend/tests/integration/test_suggestion_no_emotion.py
- [ ] T067 [P] [US3] Component test for ContextForm skip emotion functionality in frontend/tests/integration/ContextForm.test.tsx
- [ ] T068 [US3] E2E test for User Story 3 flow in frontend/tests/e2e/user-story-3.spec.ts

### Implementation for User Story 3

- [ ] T069 [US3] Update AI matching service to handle missing emotional_state in backend/src/services/ai_matching_service.py
- [ ] T070 [US3] Update ContextForm to allow skipping emotional state question in frontend/src/components/ContextForm.tsx
- [ ] T071 [US3] Update UserContext model to make emotional_state optional in backend/src/models/user_context.py
- [ ] T072 [US3] Update API validation to allow null emotional_state in backend/src/api/routes/context.py

**Checkpoint**: All user stories should now be independently functional

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T073 [P] Add comprehensive error messages and user feedback throughout frontend
- [ ] T074 [P] Add loading states and spinners for async operations in frontend components
- [ ] T075 [P] Implement request timeout handling for AI API calls in backend/src/services/ai_client.py
- [ ] T076 [P] Add retry logic for failed AI API calls in backend/src/services/ai_client.py
- [ ] T077 [P] Implement caching for AI inferences (optional SQLite cache) in backend/src/services/cache_service.py
- [ ] T078 [P] Add input sanitization for all user inputs in backend API endpoints
- [ ] T079 [P] Add comprehensive unit tests for all services in backend/tests/unit/
- [ ] T080 [P] Add comprehensive unit tests for all components in frontend/tests/unit/
- [ ] T081 [P] Update API documentation (OpenAPI spec) with examples in backend/src/api/docs/
- [ ] T082 [P] Create README.md with setup and usage instructions
- [ ] T083 [P] Add environment variable validation on startup in backend/src/config.py
- [ ] T084 [P] Add frontend environment variable validation in frontend/src/config.ts
- [ ] T085 Run quickstart.md validation to ensure all steps work
- [ ] T086 Code cleanup and refactoring across all modules
- [ ] T087 Performance optimization (check AI API response times meet SC-001 and SC-005)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - Depends on US1 TaskSuggestion component but should be independently testable
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - Depends on US1 ContextForm component but should be independently testable

### Within Each User Story

- Tests (REQUIRED) MUST be written and FAIL before implementation
- Models before services
- Services before endpoints
- Backend endpoints before frontend components
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Launch all contract tests for User Story 1 together:
Task: "Contract test for POST /api/v1/tasks/import in backend/tests/contract/test_import_tasks.py"
Task: "Contract test for POST /api/v1/context in backend/tests/contract/test_context.py"
Task: "Contract test for POST /api/v1/suggestions in backend/tests/contract/test_suggestions.py"
Task: "Contract test for POST /api/v1/tasks/{task_id}/breakdown in backend/tests/contract/test_breakdown.py"

# Launch all models for User Story 1 together:
Task: "Create UserContext Pydantic model in backend/src/models/user_context.py"
Task: "Create Task Pydantic model in backend/src/models/task.py"
Task: "Create TaskStep Pydantic model in backend/src/models/task_step.py"
Task: "Create TodoList Pydantic model in backend/src/models/todo_list.py"
Task: "Create UserContext TypeScript type in frontend/src/types/userContext.ts"
Task: "Create Task TypeScript type in frontend/src/types/task.ts"
Task: "Create TaskStep TypeScript type in frontend/src/types/taskStep.ts"
Task: "Create TodoList TypeScript type in frontend/src/types/todoList.ts"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1 (backend focus)
   - Developer B: User Story 1 (frontend focus)
   - Developer C: User Story 2 (can start after US1 models/services ready)
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing (TDD mandatory per constitution)
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence
- All API endpoints must follow OpenAPI contract in contracts/openapi.yaml
- All models must follow data-model.md definitions
- Test coverage target: 80%+ on critical paths (per constitution)
