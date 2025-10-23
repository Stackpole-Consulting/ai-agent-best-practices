# Agent Handoff Status Template

> **Stackpole Consulting AI Agent Best Practices**  
> Use this template to document work state between AI agent sessions.

## Current Session Information

**Date:** [YYYY-MM-DD]  
**Session Duration:** [Estimated time]  
**AI Agent Used:** [GitHub Copilot, Claude, ChatGPT, etc.]  
**Primary Focus:** [Brief description of main work area]

## Project State Summary

### What Was Accomplished
<!-- List completed tasks with enough detail for next session -->
- [ ] **[Task 1]:** [Brief description and outcome]
- [ ] **[Task 2]:** [Brief description and outcome]
- [ ] **[Task 3]:** [Brief description and outcome]

### Key Decisions Made
<!-- Document architectural or design decisions -->
1. **[Decision Topic]:** [What was decided and why]
2. **[Decision Topic]:** [What was decided and why]
3. **[Decision Topic]:** [What was decided and why]

### Files Modified
<!-- List all files changed with brief description -->
- `[filepath]` - [What was changed]
- `[filepath]` - [What was changed]
- `[filepath]` - [What was changed]

### New Dependencies Added
<!-- Track any new libraries, packages, or external services -->
- **[Dependency Name]** ([Version]) - [Purpose/reason for adding]
- **[Dependency Name]** ([Version]) - [Purpose/reason for adding]

## Work In Progress

### Incomplete Tasks
<!-- What's partially done but needs finishing -->
1. **[Task Description]**
   - **Status:** [How far along, what's left]
   - **Context:** [Important details the next session needs]
   - **Files Involved:** [List relevant files]

2. **[Task Description]**
   - **Status:** [How far along, what's left]
   - **Context:** [Important details the next session needs]
   - **Files Involved:** [List relevant files]

### Known Issues/Blockers
<!-- Problems that need resolution -->
- **[Issue Description]:** [What's wrong and potential solutions considered]
- **[Issue Description]:** [What's wrong and potential solutions considered]

### Pending Decisions
<!-- Choices that need to be made -->
- **[Decision Needed]:** [Options being considered, pros/cons]
- **[Decision Needed]:** [Options being considered, pros/cons]

## Next Session Priorities

### Immediate Tasks (Next 1-2 sessions)
1. **[High Priority Task]** - [Why it's important]
2. **[High Priority Task]** - [Why it's important]
3. **[High Priority Task]** - [Why it's important]

### Upcoming Features/Improvements
- **[Feature]** - [Brief description and estimated complexity]
- **[Feature]** - [Brief description and estimated complexity]

### Technical Debt to Address
- **[Issue]** - [Impact and suggested approach]
- **[Issue]** - [Impact and suggested approach]

## Context for Next Session

### Current Architecture State
<!-- High-level description of how the system is organized -->
[Provide a paragraph describing the current overall structure, key components, and how they interact. This helps the next session understand the big picture.]

### Recent Pattern Changes
<!-- Any new conventions or standards established -->
- **[Pattern/Convention]:** [What changed and why]
- **[Pattern/Convention]:** [What changed and why]

### Environment/Configuration Changes
<!-- Changes to setup, deployment, or development environment -->
- **[Change]:** [What was modified and impact]
- **[Change]:** [What was modified and impact]

### Test Status
<!-- Current state of testing -->
- **Unit Tests:** [Pass/fail status, coverage notes]
- **Integration Tests:** [Status and any issues]
- **Manual Testing Done:** [What was verified]
- **Testing Gaps:** [What still needs testing]

## Agent Performance Notes

### What Worked Well
<!-- Effective collaboration patterns to repeat -->
- [Specific prompting techniques or approaches that were effective]
- [Types of tasks the agent handled particularly well]

### Areas for Improvement
<!-- Challenges faced with agent collaboration -->
- [Situations where communication could be clearer]
- [Tasks that required multiple iterations]

### Context Management
<!-- How well context was maintained -->
- **Token Usage:** [Approximate level, any warnings hit]
- **Context Loss:** [Any instances where agent "forgot" earlier work]
- **Recovery Strategies:** [What worked to restore context]

## Code Quality Checklist

### Standards Compliance
- [ ] Follows established naming conventions
- [ ] Adheres to project file structure
- [ ] Includes appropriate error handling
- [ ] Has necessary input validation
- [ ] Includes relevant documentation/comments

### Performance Considerations
- [ ] No obvious performance bottlenecks introduced
- [ ] Database queries optimized (if applicable)
- [ ] Appropriate caching implemented (if applicable)
- [ ] Resource usage reasonable

### Security Review
- [ ] Input sanitization in place
- [ ] No hardcoded credentials or secrets
- [ ] Proper access controls implemented
- [ ] External API calls secured

## Commit History Summary

### Recent Commits
```
[Paste recent git log entries or summarize key commits]
git log --oneline -10
```

### Branching State
- **Current Branch:** [branch name]
- **Upstream Status:** [synced/ahead/behind]
- **Outstanding PRs:** [any pending reviews]

---

**Template Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Instructions:** Update this file at the end of each significant coding session. Keep it alongside your project documentation for easy reference when starting new sessions.