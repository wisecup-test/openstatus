# Adopt Structured Error Logging with Context Propagation: Error Logging Extract

These rules are ALWAYS ACTIVE for all error logging in HTTP request handlers, background job processors, external client integrations, retry and backoff operations, and service initialization and shutdown sequences.

### Rules

- **R-SELOG-001** MUST: Error logging must extract error messages through error interface methods rather than string formatting of error objects.

### Verify

```bash
# Discover the project's dependency manifest and identify the structured logging library
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate the lock file and resolve the exact version in use
find . -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' | head -1

# Search the codebase for error logging patterns and verify error extraction uses interface methods
grep -r 'log\.' --include='*.go' --include='*.js' --include='*.py' | grep -i 'error' | head -20

# Identify test suites that validate context propagation through logging calls
find . -name '*_test.go' -o -name '*.test.js' -o -name 'test_*.py' | xargs grep -l 'context\|correlation' 2>/dev/null | head -10

# Execute tests to confirm correlation identifiers are preserved
go test -v ./... -run TestLog 2>&1 | grep -i 'correlation\|context' || echo 'Run project test suite to verify'
```

**Accept when:**
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure
- No error logging uses string formatting (e.g., `fmt.Sprintf("%v", err)`) instead of structured field extraction

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging patterns must be reviewed for compliance with R-SELOG-001 before code acceptance.
</enforcement>