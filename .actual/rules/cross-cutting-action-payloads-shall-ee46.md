# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Action Payloads Shall

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including multi-step confirmation and cancellation flows, thread-based conversation contexts, and temporary state storage with time-bounded validity.

### Rules

- **R-CARRIER-001** MUST: Action payloads SHALL be validated against schemas before storage and after retrieval to ensure type safety across the temporal boundary.

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
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l "[Cc]arrier" 2>/dev/null | head -5

# Verify schema validation at storage boundaries
grep -r "validate.*before.*stor\|stor.*validate" --include="*.ts" --include="*.js" --include="*.py"

# Verify schema validation at retrieval boundaries
grep -r "validate.*after.*retriev\|retriev.*validate" --include="*.ts" --include="*.js" --include="*.py"
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- Code review confirms all asynchronous action coordination uses the carrier abstraction rather than direct storage access
- Static analysis confirms action payload schemas are validated at storage and retrieval boundaries
- Integration tests verify atomic consume operations prevent duplicate execution under concurrent access

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before code is committed.
</enforcement>