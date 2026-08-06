# Adopt Structured Error Logging with Context Propagation: Context Aware Logging

These rules are ALWAYS ACTIVE for all HTTP request handlers, background job processors, external client integrations, retry and backoff operations, and service initialization/shutdown sequences that perform error logging.

### Rules

- **R-CTXLOG-001** MUST: Context-aware logging must propagate request context to enable correlation with distributed tracing and request identifiers.
- **R-CTXLOG-002** MUST: Extract error messages through the error interface method rather than string formatting, ensuring structured log fields capture error details separately from log messages.
- **R-CTXLOG-003** MUST: Thread context through all operation chains including HTTP handlers, job processors, and external client calls to enable context-aware logging with correlation identifiers.
- **R-CTXLOG-004** SHOULD: Configure log levels to distinguish between transient errors in retry operations and terminal failures, preventing log volume issues while maintaining visibility into failure patterns.
- **R-CTXLOG-005** SHOULD: Implement log sampling or rate limiting for transient errors in retry loops; use lower severity levels for expected transient failures.

### Verify

```bash
# Discover the project's dependency manifest and identify the structured logging library
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate the lock file and resolve the exact version in use
find . -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' | head -1

# Search the codebase for error logging patterns
grep -r "log\.Error\|log\.Errorf\|logger\.Error" --include="*.go" --include="*.js" --include="*.py" . | head -20

# Verify that error extraction uses the error interface method rather than string formatting
grep -r "fmt\.Sprintf.*err\|fmt\.Errorf" --include="*.go" . | wc -l

# Identify test suites that validate context propagation through logging calls
find . -name '*test*.go' -o -name '*.test.js' -o -name 'test_*.py' | xargs grep -l "context\|correlation" | head -10

# Execute tests to confirm correlation identifiers are preserved
go test -v ./... -run TestContext 2>&1 | grep -E "PASS|FAIL|correlation"
```

**Accept when:**
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure
- No error logging patterns use string formatting instead of structured field extraction
- Context parameters are threaded through all HTTP handlers, job processors, and external client call chains

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be validated before code review approval.
</enforcement>