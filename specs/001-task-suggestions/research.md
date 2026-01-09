# Research: Smart Task Suggestions

**Feature**: Smart Task Suggestions  
**Date**: 2026-01-08  
**Purpose**: Resolve technical context unknowns and make technology decisions

## Technology Stack Decisions

### Backend Language & Framework

**Decision**: Python 3.11+ with FastAPI

**Rationale**:
- Strong AI/ML ecosystem with excellent libraries (OpenAI SDK, Anthropic SDK)
- FastAPI provides modern async support, automatic OpenAPI documentation, and excellent performance
- Type hints and Pydantic models enable strong validation and type safety
- Easy integration with AI APIs (OpenAI, Anthropic, etc.)
- Good testing ecosystem (pytest, httpx for async testing)
- Simpler deployment and dependency management compared to Node.js for AI workloads

**Alternatives considered**:
- **Node.js 20+ with Express**: Good for web APIs but weaker AI integration, requires more setup for AI SDKs
- **Python with Flask**: Less modern, no built-in async support, more boilerplate
- **Go**: Excellent performance but weaker AI ecosystem, more complex for rapid MVP development

### Frontend Framework

**Decision**: React 18+ with TypeScript

**Rationale**:
- Industry standard with large ecosystem and community support
- TypeScript provides type safety matching backend Pydantic models
- Excellent component libraries (shadcn/ui, Radix UI) for structured inputs
- Strong testing support (React Testing Library, Jest/Vitest)
- Good state management options (React Context, Zustand for simple state)
- Easy integration with API clients (axios, fetch)

**Alternatives considered**:
- **Vue 3**: Good alternative but smaller ecosystem, less TypeScript integration
- **Svelte**: Simpler but newer, smaller community and fewer resources
- **Vanilla JS**: Too much boilerplate, no type safety, harder to maintain

### AI/LLM Provider

**Decision**: OpenAI GPT-4 or GPT-3.5-turbo (with Anthropic Claude as fallback)

**Rationale**:
- OpenAI has excellent Python SDK, reliable API, good documentation
- GPT-3.5-turbo is cost-effective for MVP, GPT-4 for better quality if needed
- Good support for structured outputs and function calling
- Anthropic Claude as backup provides redundancy and comparison
- Both providers support task analysis, step breakdown, and matching logic

**Alternatives considered**:
- **Anthropic Claude**: Excellent quality but slightly more expensive, good as primary alternative
- **Local LLMs (Ollama, etc.)**: Too slow for MVP, requires significant infrastructure
- **Multiple providers**: Adds complexity, defer to future optimization

### Storage

**Decision**: SQLite for backend (file-based), browser localStorage for frontend session

**Rationale**:
- SQLite perfect for MVP: no database server needed, file-based, easy setup
- Browser localStorage for session data (user context, imported todos) - no backend storage needed for MVP
- Simple migration path to PostgreSQL for production
- No authentication means no user data persistence required
- Aligns with Simplicity principle - minimal infrastructure

**Alternatives considered**:
- **PostgreSQL**: Overkill for MVP, requires database server setup
- **In-memory storage**: Data loss on restart, not suitable even for MVP
- **JSON files**: No query capabilities, harder to manage

### Testing Framework

**Decision**: pytest for backend, Vitest for frontend

**Rationale**:
- pytest is Python standard, excellent fixtures and async support
- Vitest is fast, Vite-native, excellent TypeScript support, Jest-compatible API
- Both support coverage reporting (pytest-cov, @vitest/coverage-v8)
- Good integration with CI/CD pipelines
- Playwright for E2E testing (cross-browser, reliable)

**Alternatives considered**:
- **Jest**: Slower than Vitest, more configuration needed
- **unittest**: Python built-in but less features than pytest
- **Cypress**: Good but Playwright is faster and more modern

### API Design

**Decision**: RESTful API with OpenAPI 3.0 specification

**Rationale**:
- FastAPI generates OpenAPI docs automatically
- RESTful conventions are well-understood and standard
- Clear separation between frontend and backend
- Easy to test and document
- Supports future API versioning (/api/v1/)

**Alternatives considered**:
- **GraphQL**: Overkill for MVP, adds complexity, no clear benefit here
- **gRPC**: Too complex for web app, requires additional tooling

## Integration Patterns

### AI Task Matching Pattern

**Decision**: Single AI call with structured prompt to infer task properties and match

**Rationale**:
- Single API call reduces latency and cost
- Structured prompt can return JSON with task properties (duration, energy, emotion fit)
- Can rank multiple tasks in one call
- Simpler error handling than multiple calls

**Implementation approach**:
1. Send all tasks + user context to AI
2. AI returns ranked list with inferred properties
3. Select top match, exclude if already suggested
4. Fallback to quick win if no matches

### AI Task Breakdown Pattern

**Decision**: Separate AI call per task breakdown with step-by-step generation

**Rationale**:
- Task breakdown is independent operation (only when user accepts)
- Can cache breakdown results per task
- Simpler error handling (retry single task vs batch)
- Better user experience (show steps immediately when ready)

**Implementation approach**:
1. User accepts task
2. Call AI with task text + user context
3. AI returns structured steps (JSON array)
4. Display first step immediately
5. Cache steps for session

### Quick Win Task Generation

**Decision**: Predefined list with random selection, optionally enhanced by AI

**Rationale**:
- Predefined list is fast and reliable (no AI call needed)
- Can be enhanced with AI-generated variations later
- Simple to implement and test
- Aligns with Simplicity principle

**Implementation approach**:
- Hardcoded list: ["Do 10 squats", "Drink a glass of water", "Speak to a nearby friend or colleague", "Take 5 deep breaths", "Stretch your arms"]
- Random selection per session
- Future: AI can generate contextual quick wins

## Performance Considerations

### AI API Latency

**Constraint**: AI API calls must complete within 10 seconds for task suggestion (30s total budget per SC-001)

**Strategy**:
- Use GPT-3.5-turbo for faster responses (typically 2-5 seconds)
- Implement request timeout (10s) with fallback to quick win
- Cache task property inferences (same task text = same properties)
- Consider streaming responses for better perceived performance

### Frontend Performance

**Constraint**: Alternative suggestions within 5 seconds (SC-005)

**Strategy**:
- Cache task property inferences in frontend state
- Re-rank cached tasks client-side when requesting alternatives
- Only call AI if new tasks imported or context changed significantly
- Optimistic UI updates (show loading state immediately)

## Security Considerations

### Input Validation

**Decision**: Pydantic models for backend validation, TypeScript types + runtime validation for frontend

**Rationale**:
- Pydantic provides automatic validation and serialization
- TypeScript catches type errors at compile time
- Runtime validation (Zod) for frontend forms
- Prevents injection attacks and invalid data

### Data Privacy (MVP)

**Decision**: No persistent storage, all data in browser session

**Rationale**:
- MVP has no authentication, no user accounts
- Data stays in browser (localStorage)
- No backend storage of user data
- Aligns with privacy and Simplicity principles
- Future: Add encryption for sensitive data if needed

## Deployment Strategy

### MVP Deployment

**Decision**: Single server deployment (backend + frontend static files)

**Rationale**:
- Simplest deployment model
- Frontend can be served as static files from backend or CDN
- Single process to manage
- Easy to containerize (Docker)
- Future: Separate frontend/backend deployment when scaling

### Containerization

**Decision**: Docker with multi-stage builds

**Rationale**:
- Consistent deployment across environments
- Easy to run locally and in production
- Supports future CI/CD integration
- Aligns with constitution deployment requirements

## Summary

All technical context unknowns resolved:
- ✅ Backend: Python 3.11+ with FastAPI
- ✅ Frontend: React 18+ with TypeScript
- ✅ AI: OpenAI GPT-3.5-turbo/GPT-4
- ✅ Storage: SQLite (backend), localStorage (frontend)
- ✅ Testing: pytest (backend), Vitest (frontend), Playwright (E2E)
- ✅ API: RESTful with OpenAPI 3.0

Ready to proceed to Phase 1 (data model and contracts).
