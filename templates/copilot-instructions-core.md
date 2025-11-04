# AI Agent Instructions Template

> **Stackpole Consulting AI Agent Best Practices**  
> Copy this template to your project root and customize for your specific needs.

## Project Context & Architecture

### Project Overview
<!-- Customize this section for your project -->
**Project Name:** [Your Project Name]  
**Primary Language/Framework:** [e.g., Python/FastAPI, TypeScript/React, etc.]  
**Architecture Pattern:** [e.g., REST API, CLI tool, microservices, etc.]  
**Target Environment:** [e.g., Cloud deployment, local development, etc.]

### Core Business Logic
<!-- Describe what your application actually does -->
[Provide 2-3 sentences describing the core purpose and functionality of your application. This helps the AI agent understand the business context when making technical decisions.]

### Key Dependencies & Integrations
<!-- List major external systems, APIs, databases -->
- **Database:** [e.g., PostgreSQL, MongoDB, SQLite]
- **External APIs:** [e.g., Stripe, SendGrid, etc.]
- **Authentication:** [e.g., JWT, OAuth2, etc.]
- **Deployment:** [e.g., Docker, AWS Lambda, etc.]

## Development Standards

### Code Organization Principles
1. **Separation of Concerns:** Keep business logic, data access, and presentation layers distinct
2. **Single Responsibility:** Each module/class should have one clear purpose
3. **Dependency Injection:** Make dependencies explicit and testable
4. **Configuration Management:** All environment-specific values in config files

### File Structure Standards
```
[Customize this structure for your project type]
project-root/
â”œâ”€â”€ src/                    # Source code
â”œâ”€â”€ tests/                  # Test files
â”œâ”€â”€ docs/                   # Documentation
â”œâ”€â”€ config/                 # Configuration files
â”œâ”€â”€ scripts/                # Build/deployment scripts
â””â”€â”€ .ai-agent/              # AI agent instructions (this file)
```

### Naming Conventions
<!-- Adapt these to your language/framework -->
- **Files:** `kebab-case` for general files, follow language conventions for code
- **Variables:** Follow language standards (camelCase for JS/TS, snake_case for Python)
- **Functions:** Descriptive verbs (`getUserById`, `calculateTotal`, `validateInput`)
- **Classes:** Nouns representing entities (`UserService`, `PaymentProcessor`)

## Token Budget Management

### Session Monitoring
**CRITICAL:** Monitor token usage proactively to prevent context loss.

**Check token count at these natural breakpoints:**
- Before adding new major features
- After completing a significant refactor
- When switching between different modules/components
- Before starting complex debugging sessions

**Warning Signals:**
- Conversations feeling "forgetful" about earlier decisions
- AI asking for context it should remember
- Suggestions that contradict established patterns

### Context Preservation Strategy
1. **Document key decisions immediately** in commit messages and code comments
2. **Use descriptive variable/function names** that explain intent
3. **Maintain clear module boundaries** to limit context needed per conversation
4. **Create summary comments** at top of complex files

## Agent Collaboration Guidelines

### Effective Prompting
**Recommended approaches:**
- Provide specific context about what you're trying to achieve
- Mention relevant existing code/patterns when requesting changes
- Ask for explanations of suggested approaches
- Request multiple options when architectural decisions are involved

**Approaches that often lead to issues:**
- Ask for "the best way" without context
- Request changes to files you haven't shown the agent
- Assume the agent remembers details from much earlier in the conversation

### Code Quality Guidelines
Based on lessons learned from production issues:

1. **Error Handling** - Production failures often stem from unhandled edge cases
2. **Input Validation** - External inputs (APIs, user input, config) are common sources of security vulnerabilities
3. **Testability** - Untestable code becomes technical debt quickly
4. **Documentation** - Complex logic without comments becomes unmaintainable

### Architectural Decision Making
**When to extend existing code:**
- Adding similar functionality to existing modules
- Enhancing current features with related capabilities
- Building on established patterns within the codebase

**When to create new modules:**
- Introducing fundamentally different functionality
- Adding external service integrations
- Implementing cross-cutting concerns (logging, monitoring, etc.)

## Project-Specific Customizations

### Domain-Specific Patterns
<!-- Add patterns specific to your industry/domain -->
[Document any industry-specific patterns, compliance requirements, or domain conventions that the AI should follow]

### Performance Requirements
<!-- Specify performance expectations -->
- **Response Time:** [e.g., API responses < 200ms]
- **Throughput:** [e.g., Handle 1000 concurrent users]
- **Resource Limits:** [e.g., Memory usage < 512MB]

### Security Considerations
<!-- List security requirements -->
- **Data Protection:** [e.g., PII encryption, GDPR compliance]
- **Access Control:** [e.g., Role-based permissions]
- **External Communication:** [e.g., API rate limiting, input sanitization]

### Testing Strategy
<!-- Define testing approach -->
- **Unit Tests:** Cover all business logic functions
- **Integration Tests:** Test external API interactions
- **End-to-End Tests:** Validate complete user workflows
- **Performance Tests:** [If applicable]

## Session Handoff Protocol

### Before Ending a Session
1. **Document current state** in commit messages
2. **Note any pending decisions** in code comments or issues
3. **Update this file** if new patterns or requirements emerge
4. **Commit work-in-progress** with clear "WIP" markers

### Starting a New Session
1. **Review recent commits** to understand latest changes
2. **Read updated documentation** and configuration
3. **Check for any TODO/FIXME comments** added since last session
4. **Validate current state** by running tests or basic functionality

## Troubleshooting Common Issues

### Context Loss Recovery
If the AI agent seems to have lost important context:
1. Point to this instruction file
2. Reference recent commit messages for decisions made
3. Show current file structure if it has changed
4. Highlight any new dependencies or configurations

### Inconsistent Suggestions
If suggestions don't align with established patterns:
1. Reference the specific patterns established in your codebase
2. Point to similar implementations already working
3. Clarify the architectural principles driving your decisions

---

**Framework Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Last Updated:** [Date when you customize this template]  
**Next Review:** [Recommended: every 2-3 months or major version updates]
