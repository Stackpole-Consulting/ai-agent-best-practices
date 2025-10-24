# Python FastAPI Task Manager API

> **AI Agent Best Practices - Python FastAPI Example**  
> A complete REST API demonstrating framework principles in production-ready Python code

## What This Example Demonstrates

✅ **Project Organization** - Clean structure following `templates/project-organization.md`  
✅ **AI Collaboration** - Comprehensive `.copilot-instructions.md` for context  
✅ **Data Validation** - Pydantic models with layered validation from `best-practices/data-management.md`  
✅ **Testing Strategy** - Unit, integration, and E2E tests following `protocols/test-local-first.md`  
✅ **Best Practices** - All core principles in action  
✅ **Local Development** - Complete Docker Compose setup  

## Tech Stack

- **Python 3.11+** - Modern Python with type hints
- **FastAPI** - High-performance async web framework
- **PostgreSQL 15** - Relational database
- **SQLAlchemy 2.0** - ORM with async support
- **Alembic** - Database migrations
- **Pydantic V2** - Data validation
- **Pytest** - Testing framework
- **Docker & Docker Compose** - Local development environment

## Project Structure

```
python-fastapi-api/
├── src/
│   ├── api/                    # API layer
│   │   ├── v1/
│   │   │   ├── __init__.py
│   │   │   ├── tasks.py        # Task endpoints
│   │   │   └── users.py        # User endpoints
│   │   ├── dependencies.py     # FastAPI dependencies
│   │   └── middleware.py       # Custom middleware
│   ├── core/                   # Core configuration
│   │   ├── __init__.py
│   │   ├── config.py           # Pydantic settings
│   │   ├── database.py         # Database connection
│   │   └── security.py         # Auth utilities
│   ├── models/                 # SQLAlchemy models
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── task.py
│   ├── schemas/                # Pydantic schemas
│   │   ├── __init__.py
│   │   ├── user.py
│   │   └── task.py
│   ├── services/               # Business logic
│   │   ├── __init__.py
│   │   ├── user_service.py
│   │   └── task_service.py
│   └── main.py                 # Application entry point
├── tests/
│   ├── conftest.py             # Pytest fixtures
│   ├── unit/                   # Unit tests
│   │   ├── test_services.py
│   │   └── test_validation.py
│   ├── integration/            # Integration tests
│   │   └── test_api.py
│   └── e2e/                    # End-to-end tests
│       └── test_workflows.py
├── alembic/                    # Database migrations
│   ├── versions/
│   └── env.py
├── .copilot-instructions.md    # AI agent context
├── docker-compose.yml          # Local development setup
├── Dockerfile
├── requirements.txt
├── pyproject.toml
├── alembic.ini
└── README.md
```

## Quick Start

### **1. Start the Development Environment**

```bash
# Clone repository if you haven't already
git clone https://github.com/Stackpole-Consulting/ai-agent-best-practices.git
cd ai-agent-best-practices/examples/python-fastapi-api

# Start all services with Docker Compose
docker-compose up

# The API will be available at http://localhost:8000
# API documentation at http://localhost:8000/docs (Swagger UI)
# Alternative docs at http://localhost:8000/redoc
```

### **2. Run Database Migrations**

```bash
# In a new terminal (while docker-compose is running)
docker-compose exec app alembic upgrade head

# Seed development data
docker-compose exec app python scripts/seed_data.py
```

### **3. Run Tests**

```bash
# Run all tests
docker-compose exec app pytest

# Run with coverage
docker-compose exec app pytest --cov=src --cov-report=html

# Run specific test file
docker-compose exec app pytest tests/unit/test_services.py -v

# Run with watch mode for development
docker-compose exec app pytest-watch
```

### **4. Explore the API**

Visit http://localhost:8000/docs for interactive API documentation.

**Example API calls:**

```bash
# Create a user
curl -X POST http://localhost:8000/api/v1/users \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!",
    "full_name": "John Doe"
  }'

# Login
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!"
  }'

# Create a task (use token from login response)
curl -X POST http://localhost:8000/api/v1/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "title": "Implement user authentication",
    "description": "Add JWT-based auth to the API",
    "priority": "high",
    "status": "in_progress"
  }'

# Get all tasks
curl http://localhost:8000/api/v1/tasks \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

## What You'll Learn

### **1. Layered Architecture**

```python
# Request Flow:
API Route (src/api/v1/tasks.py)
    ↓
Pydantic Schema Validation (src/schemas/task.py)
    ↓
Service Layer (src/services/task_service.py)
    ↓
Database Model (src/models/task.py)
    ↓
PostgreSQL Database
```

### **2. Data Validation Pattern**

See `best-practices/data-management.md` in action:

```python
# Three layers of validation:
# 1. Pydantic schema (format, types)
# 2. Service layer (business rules)
# 3. Database constraints (data integrity)

# Example: Creating a task
# - Pydantic validates: title length, priority enum, status enum
# - Service validates: user exists, user owns task
# - Database enforces: foreign keys, unique constraints
```

### **3. Test-First Development**

```python
# tests/unit/test_task_service.py
# Tests written before implementation (TDD approach)

def test_create_task_with_valid_data():
    """AI agent can generate implementation to make this test pass"""
    task = task_service.create_task(
        user_id=user.id,
        title="Test Task",
        priority="high"
    )
    assert task.title == "Test Task"
    assert task.status == "pending"  # Default status
    assert task.created_at is not None
```

### **4. AI Agent Collaboration**

The `.copilot-instructions.md` file provides context for AI agents:

```markdown
# Example prompt that works well with this codebase:
"Add a new 'due_date' field to tasks following our existing patterns:
1. Add to Task model with SQLAlchemy
2. Add to TaskCreate and TaskUpdate schemas with Pydantic
3. Update TaskService to handle due dates
4. Add validation that due_date must be in the future
5. Add tests for the new validation
6. Create database migration"

# AI agent knows from .copilot-instructions.md:
# - Where each file type goes
# - Validation patterns we use
# - Testing approach
# - Migration command to generate
```

## Key Files to Study

### **1. `.copilot-instructions.md`**
Complete AI agent context for this project. **Start here** to understand how we guide AI to generate consistent code.

### **2. `src/services/task_service.py`**
Business logic layer showing:
- Dependency injection pattern
- Error handling
- Business rule validation
- Repository pattern usage

### **3. `src/schemas/task.py`**
Pydantic models showing:
- Input validation
- Custom validators
- Field constraints
- DTO patterns

### **4. `tests/integration/test_api.py`**
Integration tests showing:
- API testing with TestClient
- Database test fixtures
- Authentication testing
- Complete request/response validation

### **5. `docker-compose.yml`**
Local development environment showing:
- PostgreSQL setup
- Environment variables
- Volume mounts for hot reload
- Health checks

## Development Workflow

### **Adding a New Feature with AI**

Let's add a "comments" feature to tasks:

```bash
# 1. Write tests first (TDD approach)
# Create tests/unit/test_comment_service.py

# 2. Prompt AI agent:
"Create a Comment model and service following our task patterns:

REQUIREMENTS:
- Comments belong to tasks and users
- Fields: task_id, user_id, content, created_at
- Only task assignee and owner can comment
- Content must be 1-1000 characters

Follow patterns in:
- src/models/task.py (for model)
- src/services/task_service.py (for service)
- src/schemas/task.py (for schemas)
- tests/unit/test_task_service.py (for tests)

Generate:
1. src/models/comment.py
2. src/schemas/comment.py
3. src/services/comment_service.py
4. src/api/v1/comments.py
5. Database migration"

# 3. Review and test
docker-compose exec app pytest tests/unit/test_comment_service.py

# 4. Create migration
docker-compose exec app alembic revision --autogenerate -m "Add comments"
docker-compose exec app alembic upgrade head

# 5. Run full test suite
docker-compose exec app pytest
```

### **Debugging with AI**

```markdown
# When you encounter an error:
1. Copy the full error message and traceback
2. Share relevant code context
3. Ask AI to explain and fix

Example prompt:
"I'm getting this error when creating a task:

```
sqlalchemy.exc.IntegrityError: (psycopg2.errors.NotNullViolation) 
null value in column 'user_id' violates not-null constraint
```

Here's my code:
[paste code from src/services/task_service.py]

The user_id should be set from the authenticated user. Help me debug this."
```

## Common Patterns Used

### **1. Dependency Injection**

```python
# FastAPI's dependency injection system
from fastapi import Depends
from sqlalchemy.ext.asyncio import AsyncSession

async def get_db() -> AsyncSession:
    async with SessionLocal() as session:
        yield session

@router.post("/tasks")
async def create_task(
    task_data: TaskCreate,
    db: AsyncSession = Depends(get_db),  # Injected
    current_user: User = Depends(get_current_user)  # Injected
):
    return await task_service.create(db, task_data, current_user.id)
```

### **2. Error Handling**

```python
# Custom exceptions with HTTP status codes
class TaskNotFoundError(Exception):
    """Raised when task doesn't exist"""
    pass

# Middleware converts to proper HTTP response
@app.exception_handler(TaskNotFoundError)
async def task_not_found_handler(request, exc):
    return JSONResponse(
        status_code=404,
        content={"error": "task_not_found", "message": str(exc)}
    )
```

### **3. Authentication**

```python
# JWT-based authentication
from core.security import create_access_token, verify_token

# Login endpoint
@router.post("/login")
async def login(credentials: LoginCredentials):
    user = await authenticate_user(credentials.email, credentials.password)
    token = create_access_token({"sub": user.id})
    return {"access_token": token, "token_type": "bearer"}

# Protected endpoint
async def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: AsyncSession = Depends(get_db)
):
    payload = verify_token(token)
    user = await user_service.get_by_id(db, payload["sub"])
    return user
```

## Troubleshooting

### **Database Connection Issues**

```bash
# Check if PostgreSQL is running
docker-compose ps

# View database logs
docker-compose logs db

# Connect to database directly
docker-compose exec db psql -U postgres -d taskmanager

# Reset database
docker-compose down -v
docker-compose up -d db
docker-compose exec app alembic upgrade head
```

### **Migration Issues**

```bash
# View migration history
docker-compose exec app alembic history

# Downgrade one version
docker-compose exec app alembic downgrade -1

# Generate new migration
docker-compose exec app alembic revision --autogenerate -m "description"

# Apply migration
docker-compose exec app alembic upgrade head
```

### **Test Failures**

```bash
# Run with verbose output
docker-compose exec app pytest -vv

# Run specific test
docker-compose exec app pytest tests/unit/test_task_service.py::test_create_task -vv

# Run with debugger
docker-compose exec app pytest --pdb

# Clear pytest cache
docker-compose exec app pytest --cache-clear
```

## Next Steps

1. **Explore the Code**: Read through the implementation files
2. **Run the Tests**: See how comprehensive testing works
3. **Add a Feature**: Use AI to add a new endpoint following existing patterns
4. **Customize**: Adapt this example for your own use case
5. **Deploy**: Use as a starting point for production applications

## Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [SQLAlchemy 2.0 Documentation](https://docs.sqlalchemy.org/en/20/)
- [Pydantic V2 Documentation](https://docs.pydantic.dev/latest/)
- [Pytest Documentation](https://docs.pytest.org/)
- [Alembic Documentation](https://alembic.sqlalchemy.org/)

---

**Example Version:** v1.0.0  
**Framework:** Stackpole Consulting AI Agent Best Practices  
**Status:** Production-ready starting point  
**License:** MIT (same as framework)