# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Action State Updates

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including multi-step confirmation and cancellation flows, thread-based conversation contexts, and temporary state storage with time-bounded validity.

### Rules

- **R-ACTION-001** SHOULD: Action state updates SHALL support replace operations that preserve action identity while updating payload content.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Verify all action payload schemas are defined and exported
grep -r "schema" --include="*.ts" --include="*.js" --include="*.py" | grep -i "action\|payload" | head -20

# Locate the carrier interface definition and confirm operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" -A 10

# Verify carrier interface declares put, get, consume, findByThread, and replace operations
grep -E "(put|get|consume|findByThread|replace)" --include="*.ts" --include="*.js" --include="*.py" -r | grep -i carrier

# Find the test suite for carrier implementations
find . -path "*/test*" -name "*carrier*" -o -path "*/spec*" -name "*carrier*" | head -10

# Verify both memory and persistent backends pass interface contract tests
grep -r "memory.*carrier\|persistent.*carrier" --include="*.test.ts" --include="*.test.js" --include="*.spec.py" -l
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- The carrier interface explicitly declares put, get, consume, findByThread, and replace operations with appropriate type signatures
- Schema validation occurs at both storage and retrieval boundaries for all action payloads
- Atomic consume operations are verified to prevent duplicate execution under concurrent access patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All asynchronous action coordination MUST use the carrier abstraction. Direct storage access bypassing the carrier is a violation. Missing schema validation at boundaries blocks deployment. Race conditions in action processing require atomic consume implementation.
</enforcement>