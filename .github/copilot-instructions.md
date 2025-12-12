---
description: "Global instructions for the TODO application - development principles, testing approach, and workflow patterns"
---

# TODO Application Development Guide

## Project Context

This is a full-stack TODO application built with:
- **Frontend**: React with modern component patterns
- **Backend**: Express.js REST API
- **Development Philosophy**: Iterative, feedback-driven development
- **Current Phase**: Backend stabilization and frontend feature completion

## Documentation References

Consult these files to understand the project architecture and patterns:
- [docs/project-overview.md](../docs/project-overview.md) - Architecture, tech stack, and project structure
- [docs/testing-guidelines.md](../docs/testing-guidelines.md) - Test patterns and standards
- [docs/workflow-patterns.md](../docs/workflow-patterns.md) - Development workflow guidance

## Development Principles

Follow these core principles for all development work:

1. **Test-Driven Development**: Follow the Red-Green-Refactor cycle
   - Write the test first (RED)
   - Implement minimal code to pass (GREEN)
   - Refactor for quality (REFACTOR)

2. **Incremental Changes**: Make small, testable modifications
   - Each change should be independently verifiable
   - Avoid large, multi-faceted changes

3. **Systematic Debugging**: Use test failures as guides
   - Read error messages carefully
   - Isolate issues with focused tests
   - Fix root causes, not symptoms

4. **Validation Before Commit**: Ensure quality standards
   - All tests must pass
   - No lint errors
   - Manual verification for UI changes

## Testing Scope

This project uses **unit tests and integration tests ONLY**:

### Testing Tools
- **Backend**: Jest + Supertest for API testing
- **Frontend**: React Testing Library for component unit/integration tests
- **Manual Testing**: Browser testing for full UI verification

### Important Testing Constraints
- ❌ **DO NOT** suggest or implement e2e test frameworks (Playwright, Cypress, Selenium)
- ❌ **DO NOT** suggest browser automation tools
- ✅ **Reason**: Keep the lab focused on unit/integration tests without e2e complexity

### Testing Approach by Context

**Backend API changes**:
- Write Jest tests FIRST, then implement (RED-GREEN-REFACTOR)
- Use Supertest for HTTP request/response testing
- Test both success and error cases

**Frontend component features**:
- Write React Testing Library tests FIRST for component behavior, then implement (RED-GREEN-REFACTOR)
- Test user interactions, state changes, and rendering
- Follow with manual browser testing for full UI flows

**This is true TDD**: Test first, then code to pass the test.

## Workflow Patterns

### 1. TDD Workflow
The primary development workflow:
1. Write test (or fix broken test)
2. Run tests → Observe failure (RED)
3. Implement minimal code to pass (GREEN)
4. Refactor for quality while keeping tests green (REFACTOR)
5. Verify all tests pass

### 2. Code Quality Workflow
For addressing lint errors and code quality:
1. Run lint to identify issues
2. Categorize issues by severity and type
3. Fix systematically (group similar issues)
4. Re-validate with lint and tests

### 3. Integration Workflow
For end-to-end feature work:
1. Identify the issue or feature requirement
2. Debug/understand current state
3. Write tests for expected behavior
4. Implement the fix or feature
5. Verify end-to-end functionality

## Chat Mode Usage

Use specialized chat modes for focused workflows:

- **tdd-developer**: For test-related work and Red-Green-Refactor cycles
  - Writing new tests
  - Fixing failing tests
  - Implementing features with TDD approach
  - Test refactoring

- **code-reviewer**: For addressing lint errors and code quality improvements
  - Fixing lint errors
  - Code quality reviews
  - Refactoring for maintainability
  - Style consistency

## Memory System

This project uses a dual-memory architecture to track development discoveries:

- **Persistent Memory**: This file (.github/copilot-instructions.md) contains foundational principles and workflows
- **Working Memory**: .github/memory/ directory contains discoveries and patterns

### Memory Files

- **session-notes.md** (COMMITTED): Historical summaries of completed sessions
  - What was accomplished
  - Key findings and decisions
  - Outcomes and learnings
  - Updated at END of each development session

- **patterns-discovered.md** (COMMITTED): Library of recurring code patterns
  - Reusable solutions to common problems
  - Implementation templates
  - Growing knowledge base
  - Reference for consistent coding practices

- **scratch/working-notes.md** (NOT COMMITTED): Active session workspace
  - Current task tracking
  - Approach and findings during development
  - Decisions and blockers
  - Ephemeral notes cleared between sessions

### Usage During Development

**While coding**:
- Take notes in .github/memory/scratch/working-notes.md
- Track your approach, findings, and decisions
- Use it as short-term memory during TDD/debugging

**At session end**:
- Summarize key findings into .github/memory/session-notes.md
- Document reusable patterns in .github/memory/patterns-discovered.md
- Leave scratch/working-notes.md for next session (not committed)

**When providing suggestions**:
- Reference these files when providing context-aware suggestions
- Follow established patterns from patterns-discovered.md
- Learn from documented decisions in session-notes.md

See [.github/memory/README.md](memory/README.md) for complete documentation on the memory system.

## Workflow Utilities

### GitHub CLI Commands

Use these commands for workflow automation (available in all modes):

```bash
# List open issues
gh issue list --state open

# Get issue details
gh issue view <issue-number>

# Get issue with comments (includes step details)
gh issue view <issue-number> --comments
```

**Issue Structure**:
- The main exercise issue has "Exercise:" in the title
- Steps are posted as comments on the main issue
- Use these commands when `/execute-step` or `/validate-step` prompts are invoked

## Git Workflow

### Conventional Commits
Use conventional commit format for clear history:
- `feat:` - New feature
- `fix:` - Bug fix
- `chore:` - Maintenance task
- `docs:` - Documentation update
- `test:` - Test additions or modifications
- `refactor:` - Code refactoring

Example: `feat: add delete button to todo items`

### Branch Strategy
- **Feature branches**: `feature/<descriptive-name>`
- **Main branch**: `main` (protected, stable)

### Commit Process
1. Stage all changes: `git add .`
2. Commit with conventional format: `git commit -m "feat: your message"`
3. Push to correct branch: `git push origin <branch-name>`

### Best Practices
- Keep commits focused and atomic
- Write clear, descriptive commit messages
- Push frequently to avoid losing work
- Always verify branch before pushing
