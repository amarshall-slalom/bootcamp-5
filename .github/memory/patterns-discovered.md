# Discovered Code Patterns

> **Purpose**: Document recurring code patterns and solutions. This file is committed to git for team sharing and AI reference.

## Pattern Template

Use this template when documenting new patterns:

```markdown
## Pattern Name

**Context**: When does this pattern apply?

**Problem**: What issue does it solve?

**Solution**: How do we implement it?

**Example**:
```language
// Code example showing the pattern
```

**Related Files**:
- [file/path.js](../../path/to/file.js)
- [another/file.js](../../path/to/another.js)

**Notes**: Additional context or caveats
```

---

## Example Pattern

## Service Initialization Pattern

**Context**: When initializing in-memory data structures at module level in Express services

**Problem**: Uninitialized variables cause runtime errors when trying to access array/object methods

**Solution**: Always explicitly initialize collections to their empty state

**Example**:
```javascript
// ❌ DON'T: Uninitialized
let todos;  // undefined - will fail on todos.map()

// ✅ DO: Explicitly initialized
let todos = [];  // Empty array - safe to call .map(), .filter(), etc.

// ✅ DO: For objects
let todoMap = {};

// ✅ DO: For counters
let nextId = 1;
```

**Related Files**:
- [packages/backend/src/app.js](../../packages/backend/src/app.js#L8)

**Notes**:
- This is especially important for in-memory storage where no database handles initialization
- Tests may fail with "Cannot read property 'map' of undefined" if this pattern isn't followed
- Even if data will be immediately populated, initialize to empty state first

---

<!-- Add new patterns above this line -->
