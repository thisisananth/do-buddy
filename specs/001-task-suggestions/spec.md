# Feature Specification: Smart Task Suggestions

**Feature Branch**: `001-task-suggestions`  
**Created**: 2026-01-06  
**Status**: Draft  
**Input**: User description: "I'm building a website that helps users with tasks to make use of small blocks of free time during the day. Without this app they might just be scrolling social media sites. So this app works with their todo lists that they have created before. This app upon opening asks the users questions like how much time they have, what is their energy level, their emotional level (optional) and then looks into their todos and suggests a task to do. The user can accept or ask for another task. If the user accepts the app will break that task into steps and then prompt the first step for the user to get started"

## Clarifications

### Session 2026-01-06

- Q: How should the system access users' existing todo lists? → A: MVP: Manual text document import (one task per line). Future: API integration with popular todo services (Todoist, Asana, Google Tasks, etc.)
- Q: How should the system break tasks into actionable steps? → A: AI-generated breakdown using AI/LLM to analyze task text and generate steps dynamically
- Q: How should the system match tasks to user context? → A: AI-powered matching - system uses AI to infer task properties (duration, energy, emotional fit) from task text and matches to user context
- Q: What format should users use to specify time, energy, and emotional levels? → A: Structured inputs - Time: dropdown/slider (5/10/15/30/60 minutes), Energy: scale 1-5 or Low/Medium/High, Emotion: scale 1-5 or predefined moods
- Q: What should the system do when no tasks match the user's context? → A: System generates a generic quick win task suggestion (e.g., getting up and doing a simple exercise like squat, drinking a glass of water, speaking to a nearby friend or colleague)

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Get Task Suggestion and Start First Step (Priority: P1)

A user opens the website and wants to use a small block of free time productively. The system collects their context (time available, energy level, and optionally emotional state), analyzes their existing todo list, and suggests an appropriate task. The user accepts the suggestion, and the system breaks it into actionable steps and displays the first step to get them started.

**Why this priority**: This is the core value proposition of the application. Without this flow, users cannot achieve the primary goal of using small time blocks productively. This story delivers complete end-to-end value independently.

**Independent Test**: Can be fully tested by having a user with an existing todo list open the app, answer the context questions, accept a suggested task, and see the first step displayed. This delivers immediate value by helping users start productive work during available time blocks.

**Acceptance Scenarios**:

1. **Given** a user has imported their todo list (text document with one task per line) and has multiple tasks, **When** they open the website, **Then** the system prompts them to answer questions about their available time and energy level
2. **Given** a user has answered the required questions (time and energy), **When** they optionally provide their emotional state or skip it, **Then** the system uses AI to analyze task text, infer properties, match to context, and displays a suggested task
3. **Given** the system has suggested a task, **When** the user accepts the suggestion, **Then** the system uses AI to break the task into steps and displays the first step
4. **Given** a user has accepted a task suggestion, **When** they view the first step, **Then** they can see clear instructions to begin working on that step

---

### User Story 2 - Request Alternative Task Suggestion (Priority: P2)

A user receives a task suggestion but it doesn't match their current needs or preferences. They can request another suggestion, and the system provides a different task from their todo list that still matches their context (time, energy, emotional state).

**Why this priority**: Users need flexibility to find the right task for their current state. Without this, users might abandon the app if the first suggestion doesn't resonate. This enhances the core experience by providing choice and personalization.

**Independent Test**: Can be fully tested by having a user receive a task suggestion, click "suggest another task", and receive a different appropriate suggestion. This delivers value by ensuring users find tasks that match their current motivation and context.

**Acceptance Scenarios**:

1. **Given** a user has received a task suggestion, **When** they click "suggest another task" or equivalent action, **Then** the system provides a different task suggestion that matches their context
2. **Given** a user has multiple suitable tasks in their todo list, **When** they request another suggestion, **Then** the system avoids repeating recently suggested tasks
3. **Given** a user requests another suggestion, **When** there are no more suitable tasks available from their todo list, **Then** the system generates a generic quick win task suggestion (e.g., simple exercise, drinking water, speaking to nearby friend/colleague)

---

### User Story 3 - Skip Optional Emotional State Question (Priority: P3)

A user opens the website and prefers not to share their emotional state. They can skip the optional emotional level question and still receive task suggestions based on time and energy level alone.

**Why this priority**: Respecting user privacy and reducing friction is important for adoption. Some users may not want to share emotional state, and the app should still function effectively without it. This ensures the app is accessible to all users regardless of their comfort level with sharing personal information.

**Independent Test**: Can be fully tested by having a user open the app, answer time and energy questions, skip the emotional state question, and still receive appropriate task suggestions. This delivers value by maintaining functionality while respecting user privacy preferences.

**Acceptance Scenarios**:

1. **Given** a user is answering context questions, **When** they reach the optional emotional level question, **Then** they can skip it and proceed to task suggestions
2. **Given** a user has skipped the emotional state question, **When** the system suggests tasks, **Then** it uses only time and energy level for matching, and suggestions are still appropriate

---

### Edge Cases

- What happens when a user has no tasks in their todo list? → System generates a generic quick win task suggestion
- What happens when no tasks match the user's available time or energy level? → System generates a generic quick win task suggestion
- What happens when a user has very short time available (e.g., 2 minutes)?
- What happens when a user has very long time available (e.g., 4 hours)?
- How does the system handle tasks that cannot be broken into steps (e.g., AI breakdown fails or returns invalid results)?
- What happens if a user accepts a task but then closes the browser before completing it?
- How does the system handle tasks that require more time than the user has available?
- What happens when a user requests multiple alternative suggestions and runs out of suitable tasks?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST prompt users to specify their available time when they open the application using structured input (dropdown/slider with options: 5, 10, 15, 30, 60 minutes)
- **FR-002**: System MUST prompt users to specify their energy level when they open the application using structured input (scale 1-5 or Low/Medium/High options)
- **FR-003**: System MUST allow users to optionally provide their emotional state or skip this question using structured input (scale 1-5 or predefined mood options)
- **FR-004**: System MUST allow users to import their todo list via text document (one task per line) for MVP. Future versions will support API integration with popular todo services.
- **FR-005**: System MUST match tasks from the todo list to the user's context using AI-powered matching (system uses AI to infer task properties like duration, energy requirements, and emotional fit from task text, then matches to user's available time, energy level, and emotional state if provided)
- **FR-006**: System MUST suggest a single task that matches the user's context
- **FR-007**: Users MUST be able to accept a suggested task
- **FR-008**: Users MUST be able to request another task suggestion
- **FR-009**: System MUST break an accepted task into actionable steps using AI-generated breakdown (analyzing task text to generate steps dynamically)
- **FR-010**: System MUST display the first step of an accepted task to the user
- **FR-011**: System MUST avoid suggesting the same task repeatedly when user requests alternatives
- **FR-012**: System MUST handle cases where no suitable tasks match the user's context by generating a generic quick win task suggestion (e.g., simple exercise like squat, drinking water, speaking to nearby friend/colleague)
- **FR-013**: System MUST handle cases where the user has no tasks in their todo list by generating a generic quick win task suggestion

### Key Entities *(include if feature involves data)*

- **User**: Represents the person using the application. Has associated todo lists and preferences.
- **Todo List**: Collection of tasks created by the user before using this application. Contains multiple tasks that can be suggested.
- **Task**: Individual item from the user's todo list. Properties (estimated duration, required energy level, emotional fit) are inferred by AI from task text for matching against user context.
- **Task Step**: Breakdown of a task into smaller, actionable components generated by AI analysis. Each step represents a concrete action the user can take.
- **User Context**: Temporary state captured during a session, including available time (structured: 5/10/15/30/60 minutes), energy level (structured: 1-5 scale or Low/Medium/High), and optional emotional state (structured: 1-5 scale or predefined moods). Used to match and suggest appropriate tasks.

## Assumptions

- Users have existing todo lists created outside this application that the system can access
- For MVP, users will provide their todo list via text document import (one task per line)
- Future versions will support API integration with popular todo services (Todoist, Asana, Google Tasks, etc.)
- Tasks in user's todo lists can be broken down into actionable steps
- Tasks have properties (estimated duration, energy requirements, emotional fit) that are inferred by AI from task text for matching against user context
- Users understand their own energy levels and can accurately report them

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Users can receive a task suggestion within 30 seconds of opening the application
- **SC-002**: 80% of suggested tasks can be completed within the user's specified available time
- **SC-003**: Users accept at least 60% of first task suggestions without requesting alternatives
- **SC-004**: Users can complete the first step of an accepted task within 2 minutes of viewing it
- **SC-005**: System provides alternative suggestions within 5 seconds when user requests another task
- **SC-006**: 90% of users successfully receive a suggestion even when they skip the emotional state question
