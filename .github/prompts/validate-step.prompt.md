---
description: "Validate that all success criteria for the current step are met"
agent: "code-reviewer"
tools: ['codebase', 'problems', 'runCommands', 'getTerminalOutput']
---

# Validate Step Completion

You will validate that all success criteria for a specific step have been met, providing detailed feedback on completion status.

## Input

**Step number (REQUIRED)**: ${input:step-number}

Format: "X-Y" (e.g., "5-0", "5-1", "5-2")

If no step number is provided, ask the user: "Which step number should I validate? (e.g., 5-1)"

## Instructions

### 1. Find the Exercise Issue

Use gh CLI to find the main exercise issue:

```bash
gh issue list --state open
```

Look for the issue with "Exercise:" in the title. This is the main exercise issue containing all steps.

### 2. Get Issue Content with Comments

```bash
gh issue view <issue-number> --comments
```

The issue comments contain the step-by-step instructions. Each step is posted as a separate comment.

### 3. Locate the Specified Step

Search through the issue content for the step heading:

**Pattern**: `# Step ${step-number}:`

For example, if validating step "5-1", search for: `# Step 5-1:`

### 4. Extract Success Criteria

Each step has a section near the end:

```markdown
## Success Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
```

Extract all criteria from this section for the specified step.

### 5. Validate Each Criterion

For each success criterion, check the current workspace state:

#### Common Criterion Types

**"All tests pass"**:
```bash
npm test
```
Check exit code and output. Report any failing tests.

**"No ESLint errors"**:
```bash
npm run lint
```
Check for any lint errors. Report issues by file and type.

**"Specific functionality works"**:
- Check relevant code files exist
- Review implementation
- Verify tests cover the functionality
- Recommend manual testing if needed

**"Files are modified correctly"**:
- Use `codebase` tool to read relevant files
- Verify expected changes are present
- Check for proper implementation patterns

**"API endpoints implemented"**:
- Check backend route definitions
- Verify HTTP methods and paths
- Ensure proper error handling
- Review tests for endpoints

**"UI features working"**:
- Check component implementations
- Verify event handlers
- Review state management
- Recommend manual browser testing

### 6. Generate Validation Report

Provide a clear, structured report:

```markdown
## Validation Report: Step ${step-number}

### ✅ Passed Criteria (X/Y)

- ✅ All backend tests pass (24 tests)
- ✅ No ESLint errors in backend
- ✅ POST /api/todos endpoint implemented

### ❌ Incomplete Criteria (X/Y)

- ❌ Frontend tests failing (2 failures)
  → Fix: Update App.test.js to handle async data
  → File: packages/frontend/src/__tests__/App.test.js

- ❌ ESLint error in frontend
  → 1 error: Unused variable 'loading' (no-unused-vars)
  → File: packages/frontend/src/App.js:15

### 📋 Manual Verification Needed

- Manual test: Verify todo creation works in browser
  → Steps: Start app, add a todo, check if it appears in list
  → Command: npm run start

### Overall Status

✅ **Step complete - ready to commit** (if all passed)
⚠️ **Step incomplete - X issues remaining** (if any failed)
```

### 7. Provide Actionable Guidance

For any incomplete criteria:
- Identify the specific issue
- Point to the file and line number
- Suggest a fix or next action
- Reference relevant documentation if helpful

### 8. Recommend Next Steps

**If all criteria passed**:
```
✅ All success criteria met!

Next steps:
1. Run /commit-and-push <branch-name> to save your work
2. Continue to the next step
```

**If criteria incomplete**:
```
⚠️ Step not complete yet.

Recommended approach:
1. Fix [specific issue] in [file]
2. Re-run tests: npm test
3. Run /validate-step ${step-number} again
```

## Example Execution Flow

```
Input: step-number = "5-1"

🔍 Finding exercise issue...
   Found: Issue #1 "Exercise: Session 5 - Agentic Development"

📋 Loading step 5-1...
   Step 5-1: Fix Backend Tests and Implement POST

✅ Success Criteria (4 total):
   1. All backend tests pass
   2. No ESLint errors in backend
   3. POST /api/todos endpoint implemented
   4. Todos array properly initialized

🔬 Validating criterion 1: All backend tests pass
   Running: npm test --prefix packages/backend
   Result: ✅ 24 tests passed

🔬 Validating criterion 2: No ESLint errors
   Running: npm run lint --prefix packages/backend
   Result: ❌ 2 errors found
   - packages/backend/src/app.js:5 - Unused var 'debug'
   - packages/backend/src/app.js:78 - Unexpected console.log

🔬 Validating criterion 3: POST endpoint implemented
   Checking: packages/backend/src/app.js
   Result: ✅ POST /api/todos exists with proper logic

🔬 Validating criterion 4: Todos array initialized
   Checking: packages/backend/src/app.js line 3
   Result: ✅ let todos = []

📊 Validation Report
   ✅ Passed: 3/4
   ❌ Failed: 1/4

   Issue: ESLint errors in backend
   Action: Remove unused variable and console.log
   File: packages/backend/src/app.js

⚠️ Step not complete. Fix ESLint errors and re-validate.
```

## Success Indicators

- All success criteria checked against workspace state
- Clear pass/fail for each criterion
- Specific, actionable feedback for failures
- Commands provided to fix issues
- Overall completion status reported

## Remember

- Be thorough but efficient in validation
- Provide specific file paths and line numbers
- Explain WHY something doesn't meet criteria
- Give clear next steps for fixing issues
- Celebrate when all criteria pass!
- Use code-reviewer mode expertise for quality assessment
