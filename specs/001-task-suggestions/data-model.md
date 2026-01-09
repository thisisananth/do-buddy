# Data Model: Smart Task Suggestions

**Feature**: Smart Task Suggestions  
**Date**: 2026-01-08  
**Purpose**: Define entities, relationships, and validation rules

## Entities

### UserContext

Represents the temporary state captured during a user session.

**Fields**:
- `available_time_minutes` (integer, required): Available time in minutes. Valid values: 5, 10, 15, 30, 60
- `energy_level` (string or integer, required): Energy level. Valid values: "low"/"medium"/"high" or 1-5 scale
- `emotional_state` (string or integer, optional): Emotional state. Valid values: predefined moods or 1-5 scale. Can be null/undefined if skipped

**Validation Rules**:
- `available_time_minutes` MUST be one of: 5, 10, 15, 30, 60
- `energy_level` MUST be valid enum value (normalized to string: "low", "medium", "high")
- `emotional_state` is optional, but if provided MUST be valid enum value

**Storage**: Frontend only (localStorage session), not persisted to backend

**Relationships**: None (temporary session data)

### Task

Represents an individual task from the user's imported todo list.

**Fields**:
- `id` (string, required): Unique identifier for the task (generated on import)
- `text` (string, required): Task description text (from imported file, one line)
- `inferred_duration_minutes` (integer, optional): AI-inferred estimated duration in minutes
- `inferred_energy_level` (string, optional): AI-inferred energy requirement ("low", "medium", "high")
- `inferred_emotional_fit` (string, optional): AI-inferred emotional fit (mood tags)
- `suggested_count` (integer, default: 0): Number of times this task has been suggested in current session
- `last_suggested_at` (datetime, optional): Timestamp of last suggestion

**Validation Rules**:
- `text` MUST be non-empty string, max 1000 characters
- `id` MUST be unique within a session
- `inferred_duration_minutes` MUST be positive integer if present
- `inferred_energy_level` MUST be one of: "low", "medium", "high" if present
- `suggested_count` MUST be non-negative integer

**Storage**: Frontend localStorage (session), backend SQLite (optional caching)

**Relationships**: 
- Belongs to TodoList (many-to-one)
- Has many TaskSteps (one-to-many)

**State Transitions**:
- `imported` → `analyzed` (when AI infers properties)
- `analyzed` → `suggested` (when shown to user)
- `suggested` → `accepted` (when user accepts)
- `suggested` → `rejected` (when user requests another)

### TaskStep

Represents a single actionable step within a task breakdown.

**Fields**:
- `id` (string, required): Unique identifier for the step
- `task_id` (string, required): Reference to parent task
- `step_number` (integer, required): Sequential step number (1-indexed)
- `description` (string, required): Step description text
- `estimated_minutes` (integer, optional): Estimated time to complete this step

**Validation Rules**:
- `step_number` MUST be positive integer, sequential (1, 2, 3, ...)
- `description` MUST be non-empty string, max 500 characters
- `task_id` MUST reference existing task
- `estimated_minutes` MUST be positive integer if present

**Storage**: Frontend localStorage (session), backend SQLite (optional caching)

**Relationships**: 
- Belongs to Task (many-to-one)

### TodoList

Represents the collection of tasks imported by the user.

**Fields**:
- `id` (string, required): Unique identifier for the todo list (session-based)
- `tasks` (array of Task, required): List of tasks
- `imported_at` (datetime, required): Timestamp when list was imported
- `source` (string, required): Source type. Values: "text_file" (MVP)

**Validation Rules**:
- `tasks` MUST be non-empty array (at least one task)
- `source` MUST be valid enum value
- `imported_at` MUST be valid datetime

**Storage**: Frontend localStorage (session)

**Relationships**: 
- Has many Tasks (one-to-many)

### QuickWinTask

Represents a generic quick win task suggestion when no matches found.

**Fields**:
- `id` (string, required): Unique identifier
- `text` (string, required): Quick win task description
- `category` (string, required): Category. Values: "exercise", "hydration", "social", "mindfulness"

**Validation Rules**:
- `text` MUST be non-empty string
- `category` MUST be valid enum value

**Storage**: Backend hardcoded list (no database needed)

**Predefined List**:
- "Do 10 squats" (exercise)
- "Drink a glass of water" (hydration)
- "Speak to a nearby friend or colleague" (social)
- "Take 5 deep breaths" (mindfulness)
- "Stretch your arms" (exercise)

## Data Flow

### Import Flow
1. User uploads text file (one task per line)
2. System parses file, creates TodoList with Task entities
3. Tasks stored in frontend localStorage
4. Tasks sent to backend for AI property inference (optional caching)

### Suggestion Flow
1. User provides UserContext (time, energy, emotion)
2. System sends UserContext + Tasks to backend AI matching service
3. AI returns ranked tasks with inferred properties
4. System selects top match (excluding recently suggested)
5. If no matches, system selects QuickWinTask

### Breakdown Flow
1. User accepts Task
2. System sends Task text + UserContext to backend AI breakdown service
3. AI returns array of TaskSteps
4. System stores TaskSteps in frontend state
5. System displays first TaskStep

## Validation Summary

### Frontend Validation (TypeScript + Zod)
- UserContext: Enum validation for time/energy/emotion inputs
- Task import: Text parsing, empty line filtering, max length checks
- API responses: Schema validation for AI responses

### Backend Validation (Pydantic)
- API request models: UserContext, Task list, breakdown requests
- API response models: Task suggestions, TaskSteps
- Input sanitization: Prevent injection, validate enums, check ranges

## Database Schema (SQLite - Optional Caching)

```sql
-- Optional: Cache AI inferences to reduce API calls
CREATE TABLE task_cache (
    task_text_hash TEXT PRIMARY KEY,
    inferred_duration INTEGER,
    inferred_energy TEXT,
    inferred_emotional_fit TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE breakdown_cache (
    task_text_hash TEXT PRIMARY KEY,
    steps_json TEXT,  -- JSON array of steps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Note**: MVP uses frontend localStorage primarily. Backend caching is optional optimization.

## Session Data Structure (localStorage)

```json
{
  "todoList": {
    "id": "session-123",
    "tasks": [
      {
        "id": "task-1",
        "text": "Review project proposal",
        "inferred_duration_minutes": 30,
        "inferred_energy_level": "medium",
        "suggested_count": 0
      }
    ],
    "imported_at": "2026-01-08T10:00:00Z"
  },
  "userContext": {
    "available_time_minutes": 15,
    "energy_level": "medium",
    "emotional_state": null
  },
  "currentSuggestion": {
    "task": {...},
    "steps": [...]
  }
}
```
