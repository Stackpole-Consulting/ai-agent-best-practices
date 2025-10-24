# Examples

> **Stackpole Consulting AI Agent Best Practices**  
> Working code examples demonstrating the framework in real applications

## Overview

This directory contains **complete, runnable examples** showing how to apply the AI Agent Best Practices framework in real projects. Each example is a working application that demonstrates:

- Project structure following our templates
- AI agent collaboration via `.copilot-instructions.md`
- Best practices from the framework in action
- Complete testing setup
- Local-first development environment

## Available Examples

### **1. Python FastAPI API** (`python-fastapi-api/`)
**What it demonstrates:**
- RESTful API with CRUD operations
- Pydantic validation and type safety
- SQLAlchemy ORM with PostgreSQL
- Comprehensive testing (unit, integration, E2E)
- Docker Compose local development
- Token budget monitoring in practice

**Tech Stack:**
- Python 3.11+
- FastAPI
- PostgreSQL
- SQLAlchemy
- Pytest
- Docker

**Use this example if you're building:**
- Backend APIs
- Microservices
- Data-driven applications
- Services requiring strong type safety

### **2. Node.js Express TypeScript API** (`nodejs-express-api/`)
**What it demonstrates:**
- Express.js with TypeScript
- Dependency injection pattern
- Layered architecture (routes → controllers → services → repositories)
- Error handling middleware
- JWT authentication
- Comprehensive test suite

**Tech Stack:**
- Node.js 18+
- Express.js
- TypeScript
- PostgreSQL
- Jest
- Docker

**Use this example if you're building:**
- RESTful APIs
- Server-side applications
- TypeScript projects
- Services with complex business logic

### **3. React Frontend Application** (`react-frontend-app/`)
**What it demonstrates:**
- Modern React with TypeScript
- Custom hooks pattern
- API integration with error handling
- Component testing with React Testing Library
- State management best practices
- AI-friendly component structure

**Tech Stack:**
- React 18+
- TypeScript
- React Router
- Axios
- Vite
- Vitest

**Use this example if you're building:**
- Single-page applications
- Admin dashboards
- User interfaces
- Component libraries

## Quick Start

### **Run Any Example Locally**

Each example includes a complete Docker Compose setup for immediate local development:

```bash
# Clone the repository
git clone https://github.com/Stackpole-Consulting/ai-agent-best-practices.git
cd ai-agent-best-practices/examples

# Choose an example
cd python-fastapi-api   # or nodejs-express-api, or react-frontend-app

# Start the development environment
docker-compose up

# The application will be available at http://localhost:3000
# (or port specified in example's README)
```

Each example includes:
- ✅ Complete `.copilot-instructions.md` customized for that tech stack
- ✅ Working code following all framework best practices
- ✅ Comprehensive test suite
- ✅ Local development environment (Docker Compose)
- ✅ Detailed README with setup instructions
- ✅ Example data seeding scripts

## What Each Example Teaches

### **Common Across All Examples:**
1. **Project Organization**
   - File structure from `templates/project-organization.md`
   - Consistent naming conventions
   - Clear separation of concerns

2. **AI Agent Collaboration**
   - Detailed `.copilot-instructions.md` files
   - Comment-driven development examples
   - How to prompt for consistent code generation

3. **Testing Strategy**
   - Test-first development approach
   - Unit, integration, and E2E tests
   - Local testing setup from `protocols/test-local-first.md`

4. **Development Workflow**
   - Docker Compose for local dependencies
   - Database migrations
   - Seed data for development
   - Hot reload for rapid iteration

### **Python FastAPI Specifics:**
- Pydantic models for validation
- Alembic migrations
- Pytest fixtures and parametrization
- FastAPI dependency injection
- Background tasks

### **Node.js Express Specifics:**
- TypeScript strict mode
- Custom middleware patterns
- JWT authentication flow
- Repository pattern
- Express error handling

### **React Frontend Specifics:**
- Component composition
- Custom hooks for data fetching
- Error boundary implementation
- Form handling and validation
- Testing user interactions

## Learning Path

### **For Beginners:**
1. Start with **Python FastAPI** (simplest, most explicit)
2. Review the `.copilot-instructions.md` to understand AI guidance
3. Run tests to see validation in action
4. Try modifying code with AI agent assistance

### **For Intermediate Developers:**
1. Start with **Node.js Express** (industry standard patterns)
2. Study the layered architecture
3. Experiment with TypeScript types
4. Practice test-driven development with AI

### **For Frontend Developers:**
1. Start with **React Frontend**
2. Learn custom hooks pattern
3. Study component testing approaches
4. Practice AI-assisted component development

## Using Examples with AI Agents

### **Pattern 1: Learning from Examples**
```markdown
"I want to add user authentication to my project. Show me how the 
nodejs-express-api example implements JWT authentication, then help 
me adapt that pattern for my project which uses [your stack]."
```

### **Pattern 2: Extending Examples**
```markdown
"Based on the python-fastapi-api example, add a new 'products' endpoint with:
- CRUD operations
- Category relationship
- Inventory tracking
- Follow the same patterns as the 'users' endpoint"
```

### **Pattern 3: Adapting to Your Stack**
```markdown
"I'm using Django instead of FastAPI. Help me adapt the patterns from 
the python-fastapi-api example to Django, maintaining the same:
- Project structure
- Validation approach  
- Testing strategy
- AI collaboration setup"
```

## Contributing Examples

Have an example in a different tech stack? Contributions welcome!

### **Example Criteria:**
- ✅ Complete, runnable application
- ✅ Follows framework best practices
- ✅ Includes comprehensive `.copilot-instructions.md`
- ✅ Has test coverage >80%
- ✅ Includes Docker Compose setup
- ✅ Well-documented README
- ✅ Demonstrates at least 3 framework concepts

### **Needed Examples:**
- Go (Gin or Echo framework)
- Ruby on Rails
- C# .NET Core
- Java Spring Boot
- Vue.js or Svelte frontend
- Mobile (React Native, Flutter)
- CLI tools (Python Click, Go Cobra)

## Troubleshooting

### **Docker Issues**
```bash
# Reset Docker environment
docker-compose down -v
docker-compose up --build

# View logs
docker-compose logs -f
```

### **Port Conflicts**
```bash
# Check what's using a port
netstat -ano | findstr :3000  # Windows
lsof -i :3000                  # Mac/Linux

# Change port in docker-compose.yml
ports:
  - "3001:3000"  # Use 3001 instead
```

### **Database Issues**
```bash
# Reset database
docker-compose down -v
docker-compose up -d db
docker-compose run app npm run db:migrate
docker-compose run app npm run db:seed
```

## Next Steps

1. **Try an Example**: Pick one matching your tech stack and run it locally
2. **Read the Code**: Study the implementation and comments
3. **Modify with AI**: Use an AI agent to add a feature following existing patterns
4. **Apply to Your Project**: Adapt patterns to your own codebase
5. **Share Your Experience**: Contribute improvements back to the community

---

**Examples Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Status:** Production-ready, battle-tested patterns  
**Support:** See individual example READMEs for specific guidance