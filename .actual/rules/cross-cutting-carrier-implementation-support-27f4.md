# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Carrier Implementation Support

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including multi-step confirmation and cancellation flows, thread-based conversation contexts, and temporary state storage with time-to-live constraints.

### Rules

- **R-CARRIER-001** SHOULD: The carrier implementation SHOULD support both persistent and in-memory backends to enable testing and development without external dependencies.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Verify all action payload schemas are defined and exported
grep -r "schema" --include="*.ts" --include="*.js" --include="*.py" | grep -i "action\|payload" | head -20

# Locate the carrier interface definition and confirm operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" | head -10

# Verify carrier interface declares put, get, consume, findByThread, and replace operations
grep -E "(put|get|consume|findByThread|replace)" --include="*.ts" --include="*.js" --include="*.py" -r . | grep -i carrier | head -20

# Find the test suite for carrier implementations
find . -path "*/test*" -o -path "*/__tests__/*" -o -path "*/spec/*" | grep -i carrier | head -10

# Verify both memory and persistent backends exist
find . -type f \( -name "*memory*carrier*" -o -name "*persistent*carrier*" -o -name "*in-memory*" \) | head -10
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- The carrier interface declares put, get, consume, findByThread, and replace operations with appropriate type signatures
- Both memory-based and persistent carrier implementations are present and pass the same interface contract tests

<enforcement>
Claude Code MUST NOT skip or defer verification. All asynchronous action coordination MUST use the carrier abstraction. Direct storage access bypassing the carrier is a violation. Missing schema validation at storage or retrieval boundaries blocks deployment. Race conditions in action processing require atomic consume operations.
</enforcement>