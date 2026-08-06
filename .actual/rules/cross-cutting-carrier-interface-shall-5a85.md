# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Carrier Interface Shall

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including all files implementing carrier interfaces, action payload schemas, and asynchronous user interaction flows requiring confirmation or cancellation.

### Rules

- **R-CARRIER-001** MUST: The carrier interface SHALL provide atomic consume operations that retrieve and delete action state in a single operation to prevent duplicate execution.
- **R-CARRIER-002** MUST: All asynchronous action coordination SHALL use the carrier abstraction rather than direct storage access.
- **R-CARRIER-003** MUST: Action payload schemas SHALL be validated at storage and retrieval boundaries using a validation library.
- **R-CARRIER-004** MUST: The carrier interface SHALL define a minimal contract with put, get, consume, findByThread, and replace operations.
- **R-CARRIER-005** SHOULD: Action payload schemas should be co-located with the carrier implementation to ensure consistency between storage and retrieval validation.
- **R-CARRIER-006** SHOULD: Error handling for expired or missing actions should provide user-facing messages that explain the expiration and offer clear next steps.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Verify all action payload schemas are defined and exported
grep -r "schema" --include="*.ts" --include="*.js" --include="*.py" | grep -i "action\|payload" | head -20

# Locate the carrier interface definition and confirm it declares required operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" -A 10

# Verify carrier interface declares put, get, consume, findByThread, and replace operations
grep -r "\(put\|get\|consume\|findByThread\|replace\)" --include="*.ts" --include="*.js" --include="*.py" | grep -i carrier

# Find the test suite for carrier implementations
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l "[Cc]arrier" 2>/dev/null | head -5

# Verify both memory and persistent backend implementations exist
find . -type f \( -name "*carrier*" -o -name "*action*" \) | grep -E "(memory|persistent|backend)" | head -10
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- Atomic consume operations are verified to prevent duplicate execution under concurrent access in integration tests
- All asynchronous action coordination code uses the carrier abstraction rather than direct storage access
- Schema validation occurs at both storage and retrieval boundaries

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before code using asynchronous action coordination patterns is accepted.
</enforcement>