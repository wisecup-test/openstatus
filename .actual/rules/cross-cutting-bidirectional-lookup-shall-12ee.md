# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Bidirectional Lookup Shall

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including all files implementing carrier abstractions, action payload schemas, and workflows requiring multi-step confirmation or cancellation flows.

### Rules

- **R-CARRIER-001** MUST: Bidirectional lookup SHALL be supported between action identifiers and thread identifiers to enable both action-driven and conversation-driven workflows.
- **R-CARRIER-002** MUST: The carrier interface SHALL define a minimal contract with put, get, consume, findByThread, and replace operations to enable multiple backend implementations without changing consumer code.
- **R-CARRIER-003** MUST: Action payload schemas SHALL be defined using a validation library and co-located with the carrier implementation to ensure consistency between storage and retrieval validation.
- **R-CARRIER-004** MUST: All asynchronous action coordination SHALL use the carrier abstraction rather than direct storage access.
- **R-CARRIER-005** MUST: Action payload schemas SHALL be validated at storage and retrieval boundaries.
- **R-CARRIER-006** MUST: Atomic consume operations SHALL prevent duplicate execution under concurrent access.
- **R-CARRIER-007** MUST: Actions SHALL have time-to-live constraints with automatic expiration to prevent resource leaks.
- **R-CARRIER-008** SHOULD: Error handling for expired or missing actions SHALL provide user-facing messages that explain the expiration and offer clear next steps rather than exposing internal error details.
- **R-CARRIER-009** SHOULD: Health checks for the storage backend SHALL be implemented to detect failures that could cause action state loss.
- **R-CARRIER-010** SHOULD: Schema versioning SHALL be implemented in stored payloads to support backward compatibility during schema evolution.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Verify all action payload schemas are defined and exported
grep -r "schema" --include="*.ts" --include="*.js" --include="*.py" | grep -i "action\|payload" | head -20

# Locate the carrier interface definition and confirm it declares required operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" -A 10 | grep -E "put|get|consume|findByThread|replace"

# Find the test suite for carrier implementations
find . -path "*/test*" -o -path "*/__tests__/*" | grep -i carrier

# Verify both memory and persistent backend implementations exist
find . -type f \( -name "*carrier*" -o -name "*action*" \) | grep -E "memory|persistent|backend" | head -10

# Verify schema validation at boundaries
grep -r "validate\|schema" --include="*.ts" --include="*.js" --include="*.py" | grep -E "put|get|consume|retrieve" | head -10

# Check for atomic consume operation implementation
grep -r "consume" --include="*.ts" --include="*.js" --include="*.py" -B 2 -A 5 | grep -E "atomic|transaction|lock" | head -10
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- Bidirectional lookup between action identifiers and thread identifiers is implemented and tested
- Atomic consume operations prevent duplicate execution under concurrent access
- All asynchronous action coordination code uses the carrier abstraction
- Schema validation occurs at both storage and retrieval boundaries
- Error messages for expired or missing actions are user-facing and actionable

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for asynchronous action coordination patterns. Violations must be flagged in code review and refactored before merge. Exceptions require architecture review with documented rationale and engineering team lead approval.
</enforcement>