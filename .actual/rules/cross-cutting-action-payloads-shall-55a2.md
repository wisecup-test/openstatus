# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Action Payloads Shall

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including multi-step confirmation and cancellation flows, thread-based conversation contexts, and temporary state storage with automatic expiration requirements.

### Rules

- **R-CARRIER-001** MUST: Action payloads SHALL be stored with time-to-live constraints that automatically expire stale actions.
- **R-CARRIER-002** MUST: The carrier interface SHALL define a minimal contract with put, get, consume, findByThread, and replace operations to enable multiple backend implementations without changing consumer code.
- **R-CARRIER-003** MUST: Action payload schemas SHALL be defined using a validation library and co-located with the carrier implementation to ensure consistency between storage and retrieval validation.
- **R-CARRIER-004** MUST: All asynchronous action coordination SHALL use the carrier abstraction rather than direct storage access.
- **R-CARRIER-005** MUST: Schema validation SHALL occur at storage and retrieval boundaries for all action payloads.
- **R-CARRIER-006** MUST: Atomic consume operations SHALL prevent duplicate execution under concurrent access.
- **R-CARRIER-007** SHOULD: Error handling for expired or missing actions SHOULD provide user-facing messages that explain the expiration and offer clear next steps rather than exposing internal error details.
- **R-CARRIER-008** SHOULD: Action payload schemas SHOULD support versioning to enable backward compatibility during schema evolution.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'pom.xml' -o -name 'build.gradle' | head -1

# Verify all action payload schemas are defined and exported
grep -r "export.*schema\|export.*payload" --include="*.ts" --include="*.js" --include="*.py" | grep -i action

# Locate the carrier interface definition and confirm it declares required operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" | head -5

# Verify carrier interface declares put, get, consume, findByThread, and replace operations
grep -r "put\|get\|consume\|findByThread\|replace" --include="*.ts" --include="*.js" --include="*.py" | grep -i carrier

# Find the test suite for carrier implementations
find . -path '*/test*' -name '*carrier*' -o -path '*/spec*' -name '*carrier*' | head -5

# Verify both memory and persistent backend implementations exist
find . -type f \( -name '*memory*carrier*' -o -name '*persistent*carrier*' \) | head -10

# Verify action expiration monitoring or TTL configuration
grep -r "ttl\|TTL\|expir" --include="*.ts" --include="*.js" --include="*.py" | grep -i action | head -5
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- All asynchronous action coordination code uses the carrier abstraction rather than direct storage access
- Schema validation occurs at both storage and retrieval boundaries for all action payloads
- Atomic consume operations are implemented and verified to prevent duplicate execution under concurrent access
- Error handling for expired or missing actions provides user-facing messages with clear next steps

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for asynchronous action coordination patterns. Violations must be flagged in code review and refactored before merge. Missing schema validation or race conditions detected by integration tests block deployment.
</enforcement>