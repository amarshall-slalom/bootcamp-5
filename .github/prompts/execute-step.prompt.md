---
description: "Execute instructions from the current GitHub Issue step"
agent: "tdd-developer"
tools: ['codebase', 'search', 'problems', 'editFiles', 'runCommands', 'getTerminalOutput', 'testFailure']
---

# Execute GitHub Issue Step

You will execute the instructions from a GitHub Issue step systematically, following Test-Driven Development principles.

## Input

Issue number (optional): ${input:issue-number}

If no issue number is provided, find the exercise issue automatically.

## Instructions

### 1. Find the Exercise Issue

If issue number not provided:

```bash
gh issue list --state open
```

Look for an issue with "Exercise:" in the title. This is the main exercise issue.

### 2. Get Issue Content with Comments

```bash
gh issue view <issue-number> --comments
```

The main issue contains the exercise description. Each step is posted as a comment on this issue.

### 3. Parse the Latest Step

From the issue comments, identify the most recent step that should be executed. Each step has:
- A heading like `# Step X-Y: Description`
- One or more `:keyboard: Activity:` sections with numbered instructions
- A `Success Criteria` section at the end

### 4. Execute Activities Systematically

For each `:keyboard: Activity:` section in the step:

1. Read all numbered instructions carefully
2. Plan the approach (Test-Driven Development if implementing features)
3. Execute each instruction in order
4. For new features:
   - **Write tests FIRST** (RED phase)
   - Implement to pass tests (GREEN phase)
   - Refactor (REFACTOR phase)
5. For fixing failing tests:
   - Analyze failures
   - Fix code to pass tests
   - Refactor after green
6. Run tests after each change to verify
7. Keep changes incremental and testable

### 5. Testing Scope Constraints

**IMPORTANT - Follow these testing constraints:**

- ✅ Use existing test infrastructure: Jest (backend), React Testing Library (frontend)
- ✅ Write unit tests and integration tests
- ✅ Recommend manual browser testing for complete UI flows
- ❌ **NEVER** suggest installing Playwright, Cypress, Selenium, or other e2e frameworks
- ❌ **NEVER** suggest browser automation tools

**Reason**: This project focuses on TDD with unit/integration tests only, keeping the lab focused without e2e complexity.

### 6. DO NOT Commit or Push

After completing all activities in the step:

- **DO NOT** run git commands
- **DO NOT** commit changes
- **DO NOT** push to any branch

The `/commit-and-push` prompt handles git operations separately.

### 7. Completion Report

When all activities are complete:

1. Summarize what was accomplished
2. List files that were modified
3. Confirm tests are passing (if applicable)
4. Remind the user to run `/validate-step` to check success criteria
5. If validation passes, remind to run `/commit-and-push` with their feature branch name

## Example Execution Flow

```
📋 Found Exercise Issue #1: "Exercise: Session 5 - Agentic Development"

📝 Parsing Step 5-1: Fix Backend Tests

🎯 Activity 1: Fix initialization bugs
   1. Running tests to see failures...
   2. Analyzing test failure for todos initialization...
   3. Writing fix for app.js...
   4. Running tests again... ✅ PASS

🎯 Activity 2: Implement POST endpoint
   1. Writing test for POST /api/todos...
   2. Test fails as expected (RED phase)... ✅
   3. Implementing endpoint...
   4. Running test... ✅ PASS (GREEN phase)
   5. Refactoring for clarity...
   6. Tests still pass... ✅

✅ Step 5-1 activities completed!

Modified files:
- packages/backend/src/app.js

Next steps:
1. Run /validate-step 5-1 to check success criteria
2. If validation passes, run /commit-and-push <branch-name>
```

## Success Indicators

- All :keyboard: Activity: instructions executed
- Tests are passing (if applicable)
- No breaking changes introduced
- Code follows TDD principles (test first for new features)
- Ready for validation

## Remember

- Focus on one activity at a time
- Run tests frequently
- Follow Red-Green-Refactor for new features
- Keep changes small and incremental
- Let tests guide your implementation
- DO NOT commit/push - that's a separate step
