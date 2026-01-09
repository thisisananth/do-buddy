# Quickstart Guide: Smart Task Suggestions

**Feature**: Smart Task Suggestions  
**Date**: 2026-01-08  
**Purpose**: Get the application running locally for development

## Prerequisites

- Python 3.11+ installed
- Node.js 20+ and npm installed
- OpenAI API key (or Anthropic API key as alternative)
- Git installed

## Backend Setup

### 1. Navigate to backend directory

```bash
cd backend
```

### 2. Create virtual environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

**Expected dependencies** (create `requirements.txt`):
```
fastapi==0.104.1
uvicorn[standard]==0.24.0
pydantic==2.5.0
openai==1.3.0
python-dotenv==1.0.0
httpx==0.25.0
pytest==7.4.3
pytest-asyncio==0.21.1
```

### 4. Set up environment variables

Create `.env` file in `backend/`:

```bash
OPENAI_API_KEY=your_openai_api_key_here
# Optional: Use Anthropic as fallback
ANTHROPIC_API_KEY=your_anthropic_api_key_here
LOG_LEVEL=INFO
```

### 5. Run backend server

```bash
uvicorn src.api.main:app --reload --port 8000
```

Backend should be running at `http://localhost:8000`

### 6. Verify backend

```bash
curl http://localhost:8000/api/v1/health
# Or visit http://localhost:8000/docs for Swagger UI
```

## Frontend Setup

### 1. Navigate to frontend directory

```bash
cd frontend
```

### 2. Install dependencies

```bash
npm install
```

**Expected dependencies** (create `package.json`):
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "axios": "^1.6.0",
    "zod": "^3.22.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "@vitejs/plugin-react": "^4.2.0",
    "vitest": "^1.0.0",
    "@testing-library/react": "^14.1.0"
  }
}
```

### 3. Set up environment variables

Create `.env` file in `frontend/`:

```bash
VITE_API_BASE_URL=http://localhost:8000/api/v1
```

### 4. Run frontend development server

```bash
npm run dev
```

Frontend should be running at `http://localhost:5173` (or port shown in terminal)

### 5. Verify frontend

Open browser to `http://localhost:5173`

## Running Tests

### Backend Tests

```bash
cd backend
pytest tests/ -v
```

### Frontend Tests

```bash
cd frontend
npm test
```

### End-to-End Tests

```bash
# Install Playwright if not already installed
npx playwright install

# Run E2E tests
npm run test:e2e
```

## First Run Workflow

1. **Start backend**: `cd backend && uvicorn src.api.main:app --reload`
2. **Start frontend**: `cd frontend && npm run dev`
3. **Open browser**: Navigate to `http://localhost:5173`
4. **Import todo list**: 
   - Click "Import Tasks"
   - Upload a text file with one task per line, e.g.:
     ```
     Review project proposal
     Call dentist
     Buy groceries
     Write blog post
     ```
5. **Set context**: 
   - Select available time (e.g., 15 minutes)
   - Select energy level (e.g., Medium)
   - Optionally select emotional state or skip
6. **Get suggestion**: Click "Get Suggestion"
7. **Accept task**: Review suggested task and click "Accept"
8. **View steps**: See the first step displayed

## Project Structure

```
do-buddy/
├── backend/
│   ├── src/
│   │   ├── api/          # FastAPI routes
│   │   ├── models/       # Pydantic models
│   │   └── services/    # AI matching, breakdown services
│   ├── tests/
│   ├── requirements.txt
│   └── .env
├── frontend/
│   ├── src/
│   │   ├── components/   # React components
│   │   ├── pages/        # Page components
│   │   └── services/     # API client
│   ├── tests/
│   ├── package.json
│   └── .env
└── specs/
    └── 001-task-suggestions/
        ├── contracts/    # OpenAPI spec
        └── ...
```

## API Endpoints

- `POST /api/v1/context` - Set user context
- `POST /api/v1/tasks/import` - Import todo list
- `POST /api/v1/suggestions` - Get task suggestion
- `POST /api/v1/suggestions/alternative` - Get alternative suggestion
- `POST /api/v1/tasks/{task_id}/breakdown` - Break task into steps

See `contracts/openapi.yaml` for full API documentation.

## Troubleshooting

### Backend won't start

- Check Python version: `python --version` (should be 3.11+)
- Verify virtual environment is activated
- Check `.env` file exists and has `OPENAI_API_KEY`
- Check port 8000 is not in use: `lsof -i :8000`

### Frontend won't start

- Check Node.js version: `node --version` (should be 20+)
- Delete `node_modules` and `package-lock.json`, then `npm install`
- Check `.env` file has `VITE_API_BASE_URL`

### AI API errors

- Verify API key is correct in `.env`
- Check API key has credits/quota available
- Check network connectivity
- Review backend logs for detailed error messages

### CORS errors

- Ensure backend CORS middleware allows `http://localhost:5173`
- Check `VITE_API_BASE_URL` matches backend URL

## Next Steps

1. Review `spec.md` for feature requirements
2. Review `data-model.md` for entity definitions
3. Review `contracts/openapi.yaml` for API contracts
4. Start implementing tasks from `tasks.md` (after running `/speckit.tasks`)

## Development Workflow

1. **Write tests first** (TDD): Create test, see it fail, implement, see it pass
2. **Follow API contracts**: Use OpenAPI spec as source of truth
3. **Check constitution compliance**: Ensure all principles are followed
4. **Commit frequently**: Small, focused commits with clear messages

## Resources

- FastAPI docs: https://fastapi.tiangolo.com/
- React docs: https://react.dev/
- OpenAI API docs: https://platform.openai.com/docs
- OpenAPI spec: `specs/001-task-suggestions/contracts/openapi.yaml`
