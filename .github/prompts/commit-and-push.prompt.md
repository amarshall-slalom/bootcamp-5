---
description: "Analyze changes, generate commit message, and push to feature branch"
tools: ['runCommands', 'getTerminalOutput']
---

# Commit and Push Changes

You will analyze the current workspace changes, generate a conventional commit message, and push to a feature branch.

## Input

**Branch name (REQUIRED)**: ${input:branch-name}

If no branch name is provided, ask the user for it before proceeding.

## Instructions

### 1. Verify Branch Name

If `branch-name` was not provided:
- **STOP and ask the user**: "What feature branch name should I use for this commit?"
- Wait for user response
- Do not proceed without a branch name

### 2. Analyze Changes

Run git diff to see what has changed:

```bash
git status
git diff
```

Review the changes to understand:
- Which files were modified
- What functionality was added, fixed, or changed
- The scope and impact of the changes

### 3. Generate Conventional Commit Message

Based on the changes, create a commit message following the conventional commit format:

**Format**: `<type>: <description>`

**Types**:
- `feat:` - New feature
- `fix:` - Bug fix
- `test:` - Test additions or modifications
- `refactor:` - Code refactoring
- `chore:` - Maintenance task
- `docs:` - Documentation update

**Examples**:
- `feat: add DELETE endpoint for todos`
- `fix: correct toggle always setting completed to true`
- `test: add integration tests for CRUD operations`
- `refactor: extract error handling middleware`

**Guidelines**:
- Keep description concise and clear
- Use imperative mood ("add" not "added")
- Start with lowercase (after the colon)
- No period at the end
- Be specific about what changed

### 4. Create or Switch to Feature Branch

**Check if branch exists**:

```bash
git branch --list <branch-name>
```

**If branch does NOT exist**:
```bash
git checkout -b <branch-name>
```

**If branch exists**:
```bash
git checkout <branch-name>
```

**IMPORTANT**:
- ❌ **NEVER commit to main branch**
- ✅ **ONLY use the user-provided branch name**
- ✅ **Create feature branches like**: `feature/delete-endpoint`, `fix/toggle-bug`

### 5. Stage All Changes

```bash
git add .
```

Verify what will be committed:

```bash
git status
```

### 6. Commit with Generated Message

```bash
git commit -m "<generated-commit-message>"
```

Example:
```bash
git commit -m "feat: implement POST /api/todos endpoint"
```

### 7. Push to Feature Branch

```bash
git push origin <branch-name>
```

If this is the first push for a new branch, git may suggest setting upstream:
```bash
git push --set-upstream origin <branch-name>
```

### 8. Confirm Success

After successful push, provide a summary:

```
✅ Changes committed and pushed!

Branch: feature/delete-endpoint
Commit: feat: implement DELETE endpoint for todos
Files changed: 2
  - packages/backend/src/app.js
  - packages/backend/__tests__/app.test.js

Next steps:
- Continue working on this branch, or
- Create a Pull Request to merge into main
```

## Example Execution Flow

```
Input: branch-name = "feature/step-5-1-backend"

📊 Analyzing changes...
   Modified: packages/backend/src/app.js (4 changes)
   Modified: packages/backend/__tests__/app.test.js (2 changes)

💬 Generated commit message:
   "fix: initialize todos array and implement POST endpoint"

🌳 Checking branch status...
   Branch 'feature/step-5-1-backend' does not exist.
   Creating new branch...

✅ Branch created: feature/step-5-1-backend

📦 Staging changes...
   Staged 2 files

✏️ Committing...
   [feature/step-5-1-backend a1b2c3d] fix: initialize todos array and implement POST endpoint
   2 files changed, 45 insertions(+), 5 deletions(-)

⬆️ Pushing to origin...
   To github.com:username/bootcamp-5.git
    * [new branch]      feature/step-5-1-backend -> feature/step-5-1-backend

✅ Success! Changes committed and pushed to feature/step-5-1-backend
```

## Safety Checks

Before executing:
- ✅ Verify branch name is provided
- ✅ Confirm not committing to `main` directly
- ✅ Review changes are intentional
- ✅ Generate meaningful commit message

## Error Handling

If push fails:
- Check for merge conflicts
- Verify branch name is valid
- Ensure you have push permissions
- Check network connectivity

If you need to update an existing commit:
- Use `git commit --amend` if needed
- Force push with caution: `git push --force-with-lease`

## Remember

- Conventional commits create clear, searchable history
- Feature branches keep main stable
- Always review changes before committing
- Push frequently to avoid losing work
- Descriptive commit messages help future debugging
