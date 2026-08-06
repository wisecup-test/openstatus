# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Failed Validation Retrieved

These rules are ALWAYS ACTIVE for all asynchronous action coordination patterns within the system, including multi-step confirmation and cancellation flows, thread-based conversation contexts, and temporary state storage with time-bounded validity.

### Rules

- **R-CARRIER-001** MUST: Failed validation of retrieved action payloads SHALL be logged with sufficient detail for debugging while preventing system crashes.

### Verify

```bash
# Discover the project's dependency manifest and identify the validation library used for schema definition
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Verify all action payload schemas are defined and exported
grep -r "schema" --include="*.ts" --include="*.js" --include="*.py" | grep -i "action\|payload" | head -20

# Locate the carrier interface definition and confirm it declares put, get, consume, findByThread, and replace operations
grep -r "interface.*[Cc]arrier\|class.*[Cc]arrier" --include="*.ts" --include="*.js" --include="*.py" -A 10

# Find the test suite for the carrier implementations
find . -path '*/test*' -o -path '*/spec*' | grep -i carrier | head -10

# Verify both memory and persistent backends pass the same interface contract tests
grep -r "describe\|test\|it(" --include="*.test.ts" --include="*.test.js" --include="*.spec.ts" | grep -i "carrier\|memory\|persistent" | head -20
```

**Accept when:**
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation
- Failed validation errors are logged with context (action ID, payload details, validation error reason) without crashing the system
- Error handling for expired or missing actions provides user-facing messages explaining the expiration and next steps

<enforcement>
Clause R-CARRIER-001 verification is mandatory. Code review MUST confirm that all failed action payload validations are logged with sufficient debugging detail and that validation failures do not cause system crashes. Static analysis MUST verify logging statements exist at validation boundaries. Integration tests MUST verify system resilience when validation fails.
</enforcement>