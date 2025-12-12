# Development Memory System

## Purpose

This memory system tracks patterns, decisions, and lessons learned during development. It serves as a knowledge base that grows with your project, helping both developers and AI assistants provide better context-aware suggestions.

## Memory Architecture

### Persistent Memory vs Working Memory

**Persistent Memory** (`.github/copilot-instructions.md`):
- Foundational principles and workflows
- Core development standards and patterns
- Testing philosophy and guidelines
- Committed to git and shared across team

**Working Memory** (`.github/memory/`):
- Session-specific discoveries and patterns
- Historical summaries of completed work
- Active development notes
- Mix of committed (historical) and ephemeral (active) content

## Directory Structure

```
.github/memory/
├── README.md                    # This file - explains the system
├── session-notes.md             # Historical session summaries (COMMITTED)
├── patterns-discovered.md       # Accumulated code patterns (COMMITTED)
└── scratch/
    ├── .gitignore              # Ignores all files in scratch/
    └── working-notes.md        # Active session notes (NOT COMMITTED)
```

### File Purposes

**session-notes.md** - Historical Record (COMMITTED)
- Documents completed development sessions
- Captures what was accomplished, key findings, and outcomes
- Provides searchable history of project evolution
- Updated at END of each session with summary
- Committed to git as permanent record

**patterns-discovered.md** - Pattern Library (COMMITTED)
- Documents recurring code patterns and solutions
- Templates for consistent implementation
- Growing knowledge base of "how we do things"
- Referenced by AI for context-aware suggestions
- Committed to git for team sharing

**scratch/working-notes.md** - Active Session (NOT COMMITTED)
- Live notes during active development
- Track current task, approach, findings, decisions
- Ephemeral workspace - cleared between sessions
- Acts as short-term memory during TDD/debugging
- NOT committed - local scratch space only

## When to Use Each File

### During TDD Workflow

**While writing tests (RED phase)**:
- Use `scratch/working-notes.md` to track:
  - Which test you're working on
  - Expected behavior being tested
  - Edge cases to consider

**While implementing code (GREEN phase)**:
- Use `scratch/working-notes.md` to note:
  - Implementation approach
  - Challenges encountered
  - Quick decisions made

**While refactoring (REFACTOR phase)**:
- Use `scratch/working-notes.md` to document:
  - Why refactoring was needed
  - What pattern you're applying
- If pattern is reusable, add to `patterns-discovered.md`

**At session end**:
- Summarize key learnings into `session-notes.md`
- Add any new patterns to `patterns-discovered.md`
- Clear `scratch/working-notes.md` for next session

### During Linting Workflow

**While fixing lint errors**:
- Use `scratch/working-notes.md` to track:
  - Categories of errors found
  - Systematic fix approach
  - Patterns in errors (may indicate deeper issue)

**After resolving all errors**:
- Document any recurring patterns in `patterns-discovered.md`
- Note in `session-notes.md` if linting revealed architectural issues

### During Debugging Workflow

**While investigating bugs**:
- Use `scratch/working-notes.md` extensively:
  - Current hypothesis
  - Tests performed
  - Evidence gathered
  - Theories ruled out

**After bug fix**:
- Document root cause in `session-notes.md`
- If bug reveals a pattern, add to `patterns-discovered.md`
- Example: "Toggle bug - always check for state inversion logic"

### During Feature Implementation

**Planning phase**:
- Use `scratch/working-notes.md` for:
  - Breaking feature into steps
  - API design decisions
  - Component structure planning

**Implementation phase**:
- Track progress in `scratch/working-notes.md`
- Note any deviations from plan
- Document unexpected challenges

**Completion phase**:
- Move important decisions to `session-notes.md`
- Extract reusable patterns to `patterns-discovered.md`

## How AI Reads and Applies Memory

### Context Loading

When you start a conversation, AI assistants can:
1. Read `patterns-discovered.md` for established patterns
2. Review recent `session-notes.md` entries for project context
3. Check `scratch/working-notes.md` for active task context

### Pattern Application

When suggesting code, AI can:
- Reference patterns from `patterns-discovered.md`
- Suggest solutions consistent with documented decisions
- Avoid repeating mistakes documented in session notes

### Context-Aware Suggestions

With memory system, AI provides:
- **Consistent**: Follows established patterns
- **Informed**: Aware of recent decisions
- **Efficient**: Doesn't suggest already-tried approaches

## Best Practices

### Writing Effective Session Notes

✅ **DO**:
- Be specific about what was accomplished
- Document WHY decisions were made, not just WHAT
- Include relevant file/line references
- Note challenges and how they were overcome

❌ **DON'T**:
- Write vague summaries ("fixed some bugs")
- Copy/paste entire code blocks
- Document obvious or trivial changes
- Skip documenting failed approaches (they're valuable!)

### Documenting Patterns

✅ **DO**:
- Use clear pattern names
- Explain the context and problem
- Provide concrete examples
- Link to relevant files

❌ **DON'T**:
- Document one-off solutions as patterns
- Write patterns before they've proven reusable
- Skip the "why" - context matters!

### Managing Working Notes

✅ **DO**:
- Update frequently during active development
- Be informal and rapid - it's scratch space
- Track blockers and next steps clearly
- Summarize before ending session

❌ **DON'T**:
- Try to keep it polished (it's ephemeral)
- Commit it to git (it's in .gitignore)
- Let it grow unbounded without summarizing
- Skip clearing it between major tasks

## Workflow Example

### Starting a New Session

```bash
# 1. Review recent context
cat .github/memory/session-notes.md | tail -50
cat .github/memory/patterns-discovered.md

# 2. Clear scratch space
echo "# Current Task\n\n# Approach\n\n# Key Findings" > .github/memory/scratch/working-notes.md

# 3. Begin working, updating scratch/working-notes.md as you go
```

### During Development

```markdown
<!-- In scratch/working-notes.md -->
# Current Task
Implement DELETE /api/todos/:id endpoint

# Approach
1. Parse ID from params
2. Find todo by ID
3. Return 404 if not found
4. Remove from array using filter
5. Return 204 No Content

# Key Findings
- Array filter creates new array - need to reassign
- parseInt needed for ID comparison
- Test expects 204, not 200
```

### Ending Session

```bash
# 1. Review scratch/working-notes.md
# 2. Summarize key findings into session-notes.md
# 3. Add any reusable patterns to patterns-discovered.md
# 4. Commit session-notes.md and patterns-discovered.md
# 5. working-notes.md stays local (not committed)
```

## Integration with Copilot

When working with GitHub Copilot:

1. **Reference memory in prompts**:
   ```
   "Review .github/memory/patterns-discovered.md and implement
   the delete endpoint following our established patterns."
   ```

2. **Ask Copilot to update memory**:
   ```
   "Add this error handling pattern to patterns-discovered.md"
   ```

3. **Use memory for consistency**:
   ```
   "Check patterns-discovered.md - am I following our service
   initialization pattern?"
   ```

## Summary

The memory system creates a feedback loop:
1. **Discover** patterns during development
2. **Document** in appropriate memory file
3. **Reference** in future work for consistency
4. **Refine** patterns as understanding deepens

This turns individual learning into team knowledge and helps AI assistants provide increasingly context-aware suggestions over time.

---

**Remember**: The goal isn't documentation for its own sake - it's creating a system that makes you faster and more consistent over time.
