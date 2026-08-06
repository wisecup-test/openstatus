# Adopt Structured Error Logging with Context Propagation: Error Conditions That

These rules are ALWAYS ACTIVE for all error handling paths in HTTP request handlers, background job processors, external client integrations, retry and backoff operations, and service initialization/shutdown sequences.

### Rules

- **R-ERRLOG-001** MUST: All error conditions that terminate operations or trigger fallback behavior must be logged with structured error extraction.
- **R-ERRLOG-002** MUST: Extract error messages through the error interface method rather than string formatting, ensuring structured log fields capture error details separately from log messages.
- **R-ERRLOG-003** MUST: Thread context through all operation chains including HTTP handlers, job processors, and external client calls to enable context-aware logging with correlation identifiers.
- **R-ERRLOG-004** SHOULD: Configure log levels to distinguish between transient errors in retry operations and terminal failures, preventing log volume issues while maintaining visibility into failure patterns.
- **R-ERRLOG-005** SHOULD: Implement log sampling or rate limiting for transient errors in retry loops; use lower severity levels for expected transient failures.

### Verify

```bash
# Discover the project's dependency manifest and identify the structured logging library
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate lock file and resolve exact version in use
find . -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' | head -1

# Search codebase for error logging patterns
grep -r "structured.*log" --include="*.go" --include="*.js" --include="*.py" --include="*.rb" . | grep -i error

# Verify error extraction uses interface method rather than string formatting
grep -r "Error()" --include="*.go" . | grep -i log

# Identify test suites validating context propagation
find . -name '*test*.go' -o -name '*test*.js' -o -name '*test*.py' | xargs grep -l "context\|correlation"

# Execute tests to confirm correlation identifiers are preserved
go test -v ./... -run TestContext 2>&1 | grep -i correlation
```

**Accept when:**
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure
- No error logging patterns use string formatting instead of structured field extraction
- Integration tests validate log output format and correlation identifier presence

<enforcement>
Clause R-ERRLOG-001 through R-ERRLOG-005 are mandatory. Code review checklist MUST require context propagation and structured error extraction in all error handling paths. Static analysis rules MUST detect error logging patterns that use string formatting instead of structured field extraction. Integration tests MUST validate log output format and correlation identifier presence. Violations block merge until logging patterns conform to standards.
</enforcement>