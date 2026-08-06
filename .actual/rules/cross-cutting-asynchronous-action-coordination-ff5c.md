# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Asynchronous Action Coordination

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including all asynchronous user interaction flows requiring confirmation or cancellation, multi-step workflows where action initiation and execution occur in separate request cycles, thread-based conversation contexts requiring action state correlation, and temporary state storage with automatic expiration requirements.

### Rules

- **R-CARRIER-001** MUST: All asynchronous action coordination SHALL use a carrier abstraction that separates storage concerns from business logic.
- **R-CARRIER-002** MUST: The carrier interface SHALL define a minimal contract with put, get, consume, findByThread, and replace operations to enable multiple backend implementations without changing consumer code.
- **R-CARRIER-003** MUST: Action payload schemas SHALL be defined using a validation library and co-located with the carrier implementation to ensure consistency between storage and retrieval validation.
- **R-CARRIER-004** MUST: Action payload schemas SHALL be validated at storage and retrieval boundaries.
- **R-CARRIER-005** MUST: Atomic consume operations SHALL prevent duplicate execution under concurrent access.
- **R-CARRIER-006** MUST: Error handling for expired or missing actions SHALL provide user-facing messages that explain the expiration and offer clear next steps rather than exposing internal error details.
- **R-CARRIER-007** SHOULD: Implement health checks for the storage backend and provide user-facing error messages when actions cannot be retrieved.
- **R-CARRIER-008** SHOULD: Validate TTL configuration in deployment pipelines and monitor action expiration rates with alerts on anomalies.
- **R-CARRIER-009** SHOULD: Implement schema versioning in stored payloads and maintain backward compatibility for at least one TTL period during schema changes.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Verify all action payload schemas are defined and exported
grep -r "schema" --include="*.ts" --include="*.js" --include="*.py" | grep -i "action\|payload" | head -20

# Locate the carrier interface definition and confirm it declares required operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" -A 10

# Verify carrier interface declares put, get, consume, findByThread, and replace operations
grep -r "put\|get\|consume\|findByThread\|replace" --include="*.ts" --include="*.js" --include="*.py" | grep -i "carrier\|action" | head -30

# Find the test suite for carrier implementations
find . -path '*/test*' -o -path '*/spec*' | grep -i carrier | head -10

# Verify both memory and persistent backend implementations exist
find . -type f \( -name "*carrier*" -o -name "*action*" \) | grep -E "\.(ts|js|py)$" | head -20

# Verify schema validation at boundaries
grep -r "validate\|schema" --include="*.ts" --include="*.js" --include="*.py" | grep -E "(put|get|consume|retrieve)" | head -20
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- Code review confirms all asynchronous action coordination uses the carrier abstraction rather than direct storage access
- Static analysis confirms action payload schemas are validated at storage and retrieval boundaries
- Integration tests verify atomic consume operations prevent duplicate execution under concurrent access

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before accepting any code that implements asynchronous action coordination patterns.
</enforcement>