# Customizing for Your Project

> **Stackpole Consulting AI Agent Best Practices**  
> Adapt this framework to your specific technology stack and workflow

## Overview

This framework is designed to be **adapted, not adopted wholesale**. Every project has unique needs, and these best practices should be customized to fit your specific context.

**This guide will help you:**
- Decide which templates and protocols to use
- Customize templates for your tech stack
- Integrate with your existing workflow
- Create project-specific AI agent instructions

## Quick Start Checklist

### **Phase 1: Essential Setup (15 minutes)**
```markdown
□ Copy templates/copilot-instructions-core.md to your project root as .copilot-instructions.md
□ Fill in project-specific context (name, tech stack, architecture)
□ Add your coding standards and conventions
□ Configure for your AI agent (GitHub Copilot, Claude, Cursor, etc.)
□ Test with a simple prompt to verify agent understands context
```

### **Phase 2: Protocol Integration (30 minutes)**
```markdown
□ Review protocols/ directory and select relevant ones
□ Customize token-budget-monitoring.md for your conversation patterns
□ Set up conversation-continuity.md workflow (handoff template location)
□ Configure architectural-decisions.md for your codebase patterns
□ Establish test-local-first.md environment (Docker, mocks, etc.)
```

### **Phase 3: Team Alignment (1 hour)**
```markdown
□ Review best-practices/ with your team
□ Identify anti-patterns currently in your codebase
□ Create project-specific examples for each principle
□ Document team-specific conventions
□ Set up onboarding materials for new developers
```

## Decision Tree: Which Components Do You Need?

### **Solo Developer or Small Team (1-3 people)**
```
START HERE:
├─ Essential
│  ├─ templates/copilot-instructions-core.md ✓
│  ├─ protocols/token-budget-monitoring.md ✓
│  └─ best-practices/core-principles.md ✓
│
├─ High Value
│  ├─ protocols/test-local-first.md (if using cloud services)
│  └─ best-practices/anti-patterns.md (learn from others' mistakes)
│
└─ Optional
   ├─ protocols/conversation-continuity.md (for long projects)
   └─ protocols/architectural-decisions.md (as codebase grows)
```

### **Growing Team (4-10 people)**
```
START HERE:
├─ Essential (All of the above PLUS)
│  ├─ templates/agent-handoff-status.md ✓
│  ├─ templates/project-organization.md ✓
│  └─ protocols/architectural-decisions.md ✓
│
├─ High Value
│  ├─ best-practices/data-management.md
│  ├─ best-practices/system-architecture.md
│  └─ All protocols/
│
└─ Team Specific
   └─ Create custom onboarding guide combining all templates
```

### **Established Team (10+ people)**
```
USE EVERYTHING:
├─ All templates/ as baseline
├─ All protocols/ as standards
├─ All best-practices/ for consistency
│
└─ Plus Custom Extensions:
   ├─ Team-specific coding standards
   ├─ Architecture decision records (ADRs)
   ├─ Service-level templates for microservices
   └─ Cross-team collaboration protocols
```

## Customization Guides by Technology Stack

### **Python / FastAPI / Django**

#### **Project Structure Customization**
```markdown
# .copilot-instructions.md customization

## Project Structure
```
my-python-project/
├── src/
│   ├── api/                 # FastAPI routes and endpoints
│   │   ├── __init__.py
│   │   ├── dependencies.py  # Dependency injection
│   │   ├── v1/             # API version 1
│   │   │   ├── __init__.py
│   │   │   ├── users.py
│   │   │   └── products.py
│   │   └── middleware.py   # Custom middleware
│   ├── core/               # Core business logic
│   │   ├── __init__.py
│   │   ├── config.py       # Pydantic settings
│   │   ├── security.py     # Auth utilities
│   │   └── database.py     # SQLAlchemy setup
│   ├── models/             # SQLAlchemy models
│   │   ├── __init__.py
│   │   ├── base.py
│   │   └── user.py
│   ├── schemas/            # Pydantic schemas
│   │   ├── __init__.py
│   │   └── user.py
│   ├── services/           # Business logic layer
│   │   ├── __init__.py
│   │   └── user_service.py
│   └── main.py            # Application entry point
├── tests/
│   ├── conftest.py        # Pytest fixtures
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── alembic/               # Database migrations
│   ├── versions/
│   └── env.py
├── docker-compose.yml     # Local development
├── pyproject.toml         # Poetry dependencies
└── README.md
```
```

#### **Coding Standards**
```markdown
## Python-Specific Conventions

### Type Hints (Required)
```python
# Always use type hints for function signatures
def create_user(
    email: str, 
    password: str,
    *, 
    db: Session
) -> User:
    """Create a new user with validated input.
    
    Args:
        email: User's email address (validated format)
        password: Plain text password (will be hashed)
        db: Database session (keyword-only)
    
    Returns:
        Created User model instance
        
    Raises:
        ValidationError: If email format invalid
        DuplicateError: If email already exists
    """
    pass
```

### Pydantic Models (Data Validation)
```python
from pydantic import BaseModel, EmailStr, Field, validator

class UserCreate(BaseModel):
    email: EmailStr
    password: str = Field(..., min_length=8, max_length=100)
    first_name: str = Field(..., min_length=1, max_length=50)
    last_name: str = Field(..., min_length=1, max_length=50)
    
    @validator('password')
    def password_strength(cls, v):
        if not any(c.isupper() for c in v):
            raise ValueError('Password must contain uppercase')
        if not any(c.isdigit() for c in v):
            raise ValueError('Password must contain digit')
        return v
    
    class Config:
        schema_extra = {
            "example": {
                "email": "user@example.com",
                "password": "SecurePass123",
                "first_name": "John",
                "last_name": "Doe"
            }
        }
```

### Dependency Injection Pattern
```python
# Use FastAPI's dependency injection consistently
from fastapi import Depends
from sqlalchemy.orm import Session

def get_db() -> Generator[Session, None, None]:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: Session = Depends(get_db)
) -> User:
    # Validate token and return user
    pass

# Use in route handlers
@router.post("/users")
def create_user(
    user_data: UserCreate,
    current_user: User = Depends(get_current_user),
    db: Session = Depends(get_db)
):
    # Handler logic
    pass
```
```
```

### **Node.js / Express / TypeScript**

#### **Project Structure Customization**
```markdown
## Project Structure
```
my-node-project/
├── src/
│   ├── api/                # API layer
│   │   ├── middleware/
│   │   │   ├── auth.ts
│   │   │   ├── error.ts
│   │   │   └── validation.ts
│   │   ├── routes/
│   │   │   ├── index.ts
│   │   │   ├── users.ts
│   │   │   └── products.ts
│   │   └── controllers/
│   │       ├── UserController.ts
│   │       └── ProductController.ts
│   ├── services/          # Business logic
│   │   ├── UserService.ts
│   │   └── EmailService.ts
│   ├── models/            # Data models
│   │   ├── User.ts
│   │   └── Product.ts
│   ├── repositories/      # Data access layer
│   │   ├── UserRepository.ts
│   │   └── ProductRepository.ts
│   ├── config/            # Configuration
│   │   ├── database.ts
│   │   └── app.ts
│   ├── types/             # TypeScript types
│   │   ├── express.d.ts
│   │   └── models.ts
│   ├── utils/             # Utilities
│   │   ├── logger.ts
│   │   └── validation.ts
│   └── index.ts          # Entry point
├── tests/
│   ├── setup.ts
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── docker-compose.yml
├── tsconfig.json
├── package.json
└── README.md
```
```

#### **TypeScript Configuration**
```typescript
// Add to .copilot-instructions.md

## TypeScript Standards

### Strict Type Safety
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUncheckedIndexedAccess": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}

### Interface Definitions
// Always define interfaces for data structures
interface User {
  id: string;
  email: string;
  firstName: string;
  lastName: string;
  createdAt: Date;
  updatedAt: Date;
}

interface UserCreateDTO {
  email: string;
  password: string;
  firstName: string;
  lastName: string;
}

interface UserResponseDTO {
  id: string;
  email: string;
  firstName: string;
  lastName: string;
}

### Service Layer Pattern
class UserService {
  constructor(
    private userRepository: UserRepository,
    private emailService: EmailService
  ) {}
  
  async createUser(data: UserCreateDTO): Promise<UserResponseDTO> {
    // Validate input
    this.validateUserData(data);
    
    // Check for duplicates
    const existing = await this.userRepository.findByEmail(data.email);
    if (existing) {
      throw new ConflictError('Email already registered');
    }
    
    // Create user
    const hashedPassword = await this.hashPassword(data.password);
    const user = await this.userRepository.create({
      ...data,
      password: hashedPassword
    });
    
    // Send welcome email (async, don't wait)
    this.emailService.sendWelcomeEmail(user.email).catch(err => {
      logger.error('Failed to send welcome email', { userId: user.id, error: err });
    });
    
    // Return DTO (no password)
    return this.toResponseDTO(user);
  }
  
  private toResponseDTO(user: User): UserResponseDTO {
    return {
      id: user.id,
      email: user.email,
      firstName: user.firstName,
      lastName: user.lastName
    };
  }
}
```

### **React / Next.js / Frontend**

#### **Component Organization**
```markdown
## Frontend Project Structure
```
my-react-app/
├── src/
│   ├── components/         # Reusable components
│   │   ├── common/         # Generic UI components
│   │   │   ├── Button/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Button.test.tsx
│   │   │   │   ├── Button.stories.tsx
│   │   │   │   └── Button.module.css
│   │   │   └── Input/
│   │   ├── layout/         # Layout components
│   │   │   ├── Header/
│   │   │   ├── Footer/
│   │   │   └── Sidebar/
│   │   └── features/       # Feature-specific components
│   │       ├── auth/
│   │       └── products/
│   ├── pages/             # Next.js pages or React Router routes
│   │   ├── index.tsx
│   │   ├── users/
│   │   └── products/
│   ├── hooks/             # Custom React hooks
│   │   ├── useAuth.ts
│   │   ├── useDebounce.ts
│   │   └── useLocalStorage.ts
│   ├── services/          # API clients
│   │   ├── api.ts
│   │   ├── userService.ts
│   │   └── productService.ts
│   ├── store/             # State management
│   │   ├── slices/
│   │   └── store.ts
│   ├── types/             # TypeScript types
│   │   ├── api.ts
│   │   └── models.ts
│   ├── utils/             # Utilities
│   │   ├── formatters.ts
│   │   └── validators.ts
│   └── styles/            # Global styles
│       └── globals.css
├── public/                # Static assets
├── tests/
└── package.json
```
```

#### **React Best Practices**
```typescript
// Add to .copilot-instructions.md

## React Component Standards

### Functional Components with TypeScript
interface UserProfileProps {
  userId: string;
  onUpdate?: (user: User) => void;
}

export const UserProfile: React.FC<UserProfileProps> = ({ userId, onUpdate }) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);
  
  useEffect(() => {
    loadUser();
  }, [userId]);
  
  const loadUser = async () => {
    try {
      setLoading(true);
      const data = await userService.getById(userId);
      setUser(data);
    } catch (err) {
      setError(err as Error);
    } finally {
      setLoading(false);
    }
  };
  
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;
  if (!user) return null;
  
  return (
    <div className="user-profile">
      {/* Component content */}
    </div>
  );
};

### Custom Hooks Pattern
export function useUser(userId: string) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);
  
  useEffect(() => {
    let cancelled = false;
    
    async function loadUser() {
      try {
        setLoading(true);
        const data = await userService.getById(userId);
        if (!cancelled) {
          setUser(data);
        }
      } catch (err) {
        if (!cancelled) {
          setError(err as Error);
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    }
    
    loadUser();
    
    return () => {
      cancelled = true;
    };
  }, [userId]);
  
  return { user, loading, error, refetch: loadUser };
}

// Usage
const { user, loading, error } = useUser(userId);
```

## AI Agent-Specific Customizations

### **GitHub Copilot**
```markdown
# Add to .copilot-instructions.md

## GitHub Copilot Configuration

### Inline Completion Preferences
- Prefer complete function implementations over snippets
- Include error handling in generated code
- Add TypeScript types to all suggestions
- Follow project's naming conventions

### Comment-Driven Development
// Copilot responds well to detailed comments
// Example: Generate user authentication with email/password, JWT tokens, bcrypt hashing
async function authenticateUser(email: string, password: string) {
  // Copilot will generate implementation based on this description
}
```

### **Claude / Cursor**
```markdown
# Add to project context

## Claude Collaboration Guidelines

### Provide Rich Context
When working with Claude, always provide:
1. Current file structure context
2. Related files that may be affected
3. Existing patterns to follow
4. Specific constraints or requirements

### Iterative Development
- Break large features into smaller chunks
- Review each iteration before proceeding
- Ask Claude to explain complex implementations
- Request alternatives when uncertain

### Code Review Requests
"Review this implementation for:
- Type safety issues
- Edge cases not handled
- Performance concerns
- Deviation from project patterns"
```

## Project-Specific Examples

### **Adding Domain-Specific Context**
```markdown
# .copilot-instructions.md

## Domain Knowledge: E-commerce Platform

### Business Rules
- Order total must include tax calculation based on shipping address
- Inventory is reserved when order is created, deducted when shipped
- Refunds can only be issued within 30 days of purchase
- Guest checkout allowed but account creation encouraged

### Key Entities
- **Product**: SKU, pricing, inventory, variants
- **Order**: Items, totals, shipping, payment
- **Customer**: Account info, addresses, payment methods
- **Shipment**: Tracking, carrier, status updates

### Critical Workflows
1. Checkout Process
   - Cart validation → Address entry → Payment → Order creation → Confirmation email
   
2. Order Fulfillment
   - Payment verification → Inventory check → Pick → Pack → Ship → Tracking notification

3. Returns Process
   - Request creation → Approval → Return shipping → Inspection → Refund/Exchange
```

### **Industry-Specific Compliance**
```markdown
## Compliance Requirements: Healthcare (HIPAA)

### Data Handling
- All PHI (Protected Health Information) must be encrypted at rest and in transit
- Access logging required for all PHI access
- Data retention: 7 years minimum
- Audit trails cannot be modified or deleted

### Code Patterns
// Always use for PHI access
@AuditLog(action="VIEW_PHI", resourceType="Patient")
@RequirePermission("patient:read")
async function getPatientRecord(patientId: string, userId: string) {
  // Implementation
}

### Testing Requirements
- No real PHI in test environments
- Use synthetic test data generators
- Sanitize all logs before viewing
```

## Integration with Existing Workflow

### **Git Workflow Integration**
```bash
# .git/hooks/pre-commit
#!/bin/bash
# Remind developers about AI agent handoff

if git diff --cached --name-only | grep -q "src/"; then
  echo "📝 Reminder: Update .copilot-instructions.md if:"
  echo "   - New patterns introduced"
  echo "   - Architecture changes made"
  echo "   - Coding standards updated"
  echo ""
  echo "📊 Consider creating agent-handoff-status.md if:"
  echo "   - Significant work in progress"
  echo "   - Complex refactoring underway"
  echo "   - Multiple features being developed"
  echo ""
fi
```

### **CI/CD Integration**
```yaml
# .github/workflows/ai-agent-validation.yml
name: AI Agent Context Validation

on: [pull_request]

jobs:
  validate-context:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Check for outdated AI instructions
        run: |
          # Check if code changes warrant instruction updates
          if [ $(git diff main...HEAD --name-only | wc -l) -gt 20 ]; then
            echo "⚠️ Large changeset detected. Consider updating .copilot-instructions.md"
          fi
      
      - name: Validate documentation completeness
        run: |
          # Ensure README reflects current architecture
          if git diff main...HEAD --name-only | grep -q "src/"; then
            if ! git diff main...HEAD --name-only | grep -q "README.md"; then
              echo "❌ Source code changed but README not updated"
              exit 1
            fi
          fi
```

## Success Metrics

### **Track Framework Adoption**
```markdown
## Measuring Effectiveness

### Developer Velocity
- Time to onboard new developers (target: <1 day)
- Time to implement new features (track before/after)
- Code review iterations (fewer iterations = better context)

### Code Quality
- Reduction in bugs caught in production
- Test coverage improvement
- Consistency score (linting, formatting compliance)

### AI Agent Efficiency
- Successful first-attempt implementations
- Reduction in "that's not what I meant" iterations
- Quality of generated code (passes tests without modification)

### Team Satisfaction
- Developer confidence in AI-generated code
- Time saved vs manual coding
- Frustration levels with context loss
```

## Next Steps

1. **Start Small**: Implement just `.copilot-instructions.md` first
2. **Iterate**: Add protocols as you encounter their need
3. **Customize**: Adapt examples to your domain and tech stack
4. **Share**: Contribute your customizations back to the community
5. **Evolve**: Update as your project and team grow

---

**Remember:** This framework is a starting point, not a destination. Make it yours!

---

**Customization Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Philosophy:** Adapt to your context, don't force-fit generic solutions  
**Next:** Review `guides/building-your-framework.md` to create your own innovations