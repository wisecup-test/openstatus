# Adopt Structured Error Logging with Context Propagation: Non Critical Errors

These rules are ALWAYS ACTIVE for all error logging in HTTP request handlers, background job processors, external client integrations, retry and backoff operations, and service initialization/shutdown sequences.

### Rules

- **R-NONCRIT-001** MAY: Non-critical errors in retry loops may be logged at lower severity levels to distinguish transient from terminal failures.
- **R-NONCRIT-002** MUST: Extract error messages through the error interface method rather than string formatting, ensuring structured log fields capture error details separately from log messages.
- **R-NONCRIT-003** MUST: Thread context through all operation chains including HTTP handlers, job processors, and external client calls to enable context-aware logging with correlation identifiers.
- **R-NONCRIT-004** MUST: Configure log levels to distinguish between transient errors in retry operations and terminal failures, preventing log volume issues while maintaining visibility into failure patterns.

### Verify

```bash
# Discover the project's dependency manifest and identify the structured logging library
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate lock file and resolve exact version in use
find . -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' | head -1

# Search codebase for error logging patterns
grep -r "log\.Error\|log\.Warn\|logger\.Error" --include="*.go" --include="*.js" --include="*.py" | head -20

# Verify error extraction uses interface method not string formatting
grep -r "fmt\.Sprintf.*err\|fmt\.Printf.*err" --include="*.go" --include="*.js" --include="*.py" | wc -l

# Identify test suites validating context propagation
find . -name '*test*.go' -o -name '*.test.js' -o -name 'test_*.py' | xargs grep -l "context\|correlation" | head -10

# Execute context propagation tests
go test -v ./... -run Context 2>&1 | grep -E "PASS|FAIL|correlation"
```

**Accept when:**
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure
- No error logging uses string formatting (fmt.Sprintf/Printf) for error details
- All retry loops and transient error paths use lower severity levels than terminal failures

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-NONCRIT-001 through R-NONCRIT-004 must be validated before accepting changes to error logging patterns.
</enforcement>