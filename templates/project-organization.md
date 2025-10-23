# Project Organization Template

> **Stackpole Consulting AI Agent Best Practices**  
> Establish consistent file structure and organization standards for AI agent collaboration.

## Core Organization Principles

### Predictable Structure
Your project structure should be intuitive enough that an AI agent can:
- Locate relevant files without extensive exploration
- Understand module relationships from directory layout
- Predict where new functionality should be placed
- Navigate between related components efficiently

### Separation of Concerns
Each directory should have a clear, single responsibility:
- **Source code** separate from **configuration**
- **Business logic** separate from **infrastructure**
- **Tests** mirror source structure but remain independent
- **Documentation** organized by audience and purpose

## Universal Directory Structure

### Recommended Base Structure
```
project-root/
├── .ai-agent/                    # AI agent instructions and handoff docs
│   ├── copilot-instructions.md  # Main agent guidance (from template)
│   ├── handoff-status.md        # Session state tracking (from template)
│   └── project-context.md       # Project-specific context notes
├── src/                          # All source code
├── tests/                        # Test files (mirrors src structure)
├── docs/                         # Documentation
├── config/                       # Configuration files
├── scripts/                      # Build, deployment, utility scripts
├── .gitignore                    # Version control exclusions
├── README.md                     # Project overview and setup
├── CHANGELOG.md                  # Version history
└── [language-specific files]    # package.json, requirements.txt, etc.
```

### The `.ai-agent/` Directory
**Purpose:** Centralized location for all AI agent collaboration files.

**Why Separate Directory:**
- Keeps agent instructions organized and discoverable
- Prevents cluttering the project root
- Makes it easy to exclude from production builds
- Provides consistent location across all your projects

**Recommended Contents:**
- `copilot-instructions.md` - Core agent guidance (use provided template)
- `handoff-status.md` - Session state tracking (use provided template)
- `project-context.md` - Additional project-specific context
- `decision-log.md` - Record of major architectural decisions

## Language-Specific Adaptations

### Python Projects
```
project-root/
├── .ai-agent/
├── src/
│   ├── [package_name]/
│   │   ├── __init__.py
│   │   ├── main.py              # Entry point
│   │   ├── models/              # Data models
│   │   ├── services/            # Business logic
│   │   ├── api/                 # API endpoints (if applicable)
│   │   └── utils/               # Utility functions
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── requirements.txt             # Dependencies
├── requirements-dev.txt         # Development dependencies
├── setup.py                     # Package configuration
└── pyproject.toml              # Modern Python configuration
```

### Node.js/TypeScript Projects
```
project-root/
├── .ai-agent/
├── src/
│   ├── index.ts                 # Entry point
│   ├── types/                   # Type definitions
│   ├── services/                # Business logic
│   ├── routes/                  # API routes (if applicable)
│   ├── middleware/              # Express middleware (if applicable)
│   └── utils/                   # Utility functions
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── dist/                        # Compiled output
├── package.json                 # Dependencies and scripts
├── tsconfig.json               # TypeScript configuration
└── jest.config.js              # Test configuration
```

### React/Frontend Projects
```
project-root/
├── .ai-agent/
├── src/
│   ├── components/              # Reusable UI components
│   │   ├── common/              # Generic components
│   │   └── [feature]/           # Feature-specific components
│   ├── pages/                   # Page components or views
│   ├── hooks/                   # Custom React hooks
│   ├── services/                # API calls and business logic
│   ├── store/                   # State management (Redux, Zustand, etc.)
│   ├── types/                   # TypeScript type definitions
│   ├── utils/                   # Utility functions
│   └── assets/                  # Static assets
├── public/                      # Public static files
├── tests/                       # Test files
└── package.json
```

### CLI Tools
```
project-root/
├── .ai-agent/
├── src/
│   ├── commands/                # CLI command implementations
│   ├── lib/                     # Core library functions
│   ├── utils/                   # Utility functions
│   └── index.js                 # CLI entry point
├── bin/                         # Executable scripts
├── tests/
└── package.json
```

## File Naming Conventions

### General Guidelines
- **Consistency:** Use the same naming pattern throughout the project
- **Descriptive:** Names should clearly indicate purpose
- **Language Conventions:** Follow established patterns for your language
- **Avoid Abbreviations:** Prefer `userService.js` over `usrSvc.js`

### Recommended Patterns

#### Configuration Files
- `config/database.json` - Database connection settings
- `config/app.json` - Application configuration
- `config/environment.dev.json` - Environment-specific settings
- `.env.example` - Environment variable template

#### Source Files
- `services/userService.js` - Business logic for user operations
- `models/User.js` - Data model definitions
- `controllers/authController.js` - Request handling logic
- `utils/validation.js` - Utility functions
- `types/ApiTypes.ts` - Type definitions

#### Test Files
- `tests/unit/userService.test.js` - Unit tests
- `tests/integration/api.test.js` - Integration tests
- `tests/fixtures/userData.json` - Test data
- `tests/helpers/testUtils.js` - Test utilities

## Documentation Organization

### Documentation Structure
```
docs/
├── README.md                    # Overview and quick start
├── api/                         # API documentation
│   ├── endpoints.md
│   └── authentication.md
├── deployment/                  # Deployment guides
│   ├── local-setup.md
│   └── production.md
├── development/                 # Development guides
│   ├── getting-started.md
│   ├── coding-standards.md
│   └── testing.md
├── architecture/                # System design
│   ├── overview.md
│   └── decisions/               # Architecture Decision Records
└── troubleshooting/             # Common issues and solutions
```

### Documentation Standards
- **Markdown Format:** Use `.md` files for all documentation
- **Clear Hierarchy:** Organize by audience (users, developers, ops)
- **Living Documents:** Keep documentation updated with code changes
- **Examples:** Include practical examples in all guides

## Agent-Friendly Practices

### Code Organization for AI Understanding
1. **Consistent Naming:** Use patterns that AI can learn and predict
2. **Clear Module Boundaries:** Each file should have a focused purpose
3. **Explicit Dependencies:** Make imports/requires clear and organized
4. **Logical Grouping:** Related functionality should be physically close

### Documentation for AI Context
1. **README in Each Major Directory:** Explain the purpose and contents
2. **Header Comments:** Start complex files with purpose and overview
3. **Architecture Decision Records:** Document why choices were made
4. **Inline Comments:** Explain complex business logic

### File Structure Signals
- **Parallel Structure:** Tests mirror source code organization
- **Feature Grouping:** Related files should be grouped together
- **Clear Entry Points:** Make it obvious where execution begins
- **Separation Layers:** Distinguish between data, business logic, and presentation

## Maintenance and Evolution

### Regular Reviews
- **Monthly Structure Review:** Assess if organization still serves the project
- **New Feature Planning:** Consider structure impact before adding features
- **Refactoring Opportunities:** Identify when files have outgrown their location
- **Documentation Updates:** Keep organization docs current with actual structure

### Growth Management
- **Split When Necessary:** Large files/directories should be broken down
- **Maintain Consistency:** New additions should follow established patterns
- **Update Instructions:** Modify AI agent instructions when patterns change
- **Version Documentation:** Track significant organizational changes

### Anti-Patterns to Avoid
- **Junk Drawer Directories:** Avoid `misc/`, `other/`, or `utils/` that become catch-alls
- **Deep Nesting:** Limit directory depth to 4-5 levels maximum
- **Inconsistent Naming:** Don't mix camelCase and kebab-case in the same project
- **Orphaned Files:** Ensure every file has a clear purpose and location logic

---

**Template Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Customization Notes:** Adapt the language-specific sections to match your project type. The core principles remain consistent across all project types.