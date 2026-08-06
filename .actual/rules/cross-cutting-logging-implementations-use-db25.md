# Adopt Structured Error Logging with Context Propagation: Logging Implementations Use

These rules are ALWAYS ACTIVE for all error logging sites in HTTP request handlers, background job processors, external client integrations, retry and backoff operations, and service initialization/shutdown sequences.

### Rules

- **R-LOG-001** SHOULD: Logging implementations should use structured logging libraries that support leveled output and machine-readable formats.
- **R-LOG-002** MUST: Extract error messages through the error interface method rather than string formatting, ensuring structured log fields capture error details separately from log messages.
- **R-LOG-003** MUST: Thread context through all operation chains including HTTP handlers, job processors, and external client calls to enable context-aware logging with correlation identifiers.
- **R-LOG-004** SHOULD: Configure log levels to distinguish between transient errors in retry operations and terminal failures, preventing log volume issues while maintaining visibility into failure patterns.
- **R-LOG-005** SHOULD: Implement log sampling or rate limiting for transient errors in retry loops; use lower severity levels for expected transient failures.

### Verify

```bash
# Discover the project's dependency manifest and identify the structured logging library
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate lock file and resolve exact version in use
find . -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' | head -1

# Search codebase for error logging patterns
grep -r 'log\.' --include='*.go' --include='*.js' --include='*.py' | grep -i 'error\|err' | head -20

# Verify error extraction uses interface method rather than string formatting
grep -r 'Error()' --include='*.go' --include='*.js' --include='*.py' | grep -v 'test' | head -20

# Identify test suites validating context propagation
find . -name '*test*.go' -o -name '*test*.js' -o -name '*test*.py' | xargs grep -l 'context\|correlation' | head -10

# Execute context propagation tests
go test -v ./... -run Context 2>&1 | grep -E 'PASS|FAIL|correlation'
```

**Accept when:**
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure
- Error extraction patterns consistently use the error interface method across all error handling paths
- Correlation identifiers are present in logs for all distributed operations validated by integration tests

<enforcement>
Clause MUST NOT skip or defer verification. Code review checklist requires context propagation and structured error extraction in all error handling paths. Static analysis rules detect error logging patterns that use string formatting instead of structured field extraction. Integration tests validate log output format and correlation identifier presence. Violations block merge until logging patterns conform to standards.
</enforcement>