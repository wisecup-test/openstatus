# Adopt Structured Error Logging with Context Propagation: Error Logs Include

These rules are ALWAYS ACTIVE for all error logging in HTTP request handlers, background job processors, external client integrations, retry and backoff operations, and service initialization/shutdown sequences.

### Rules

- **R-ERRLOG-001** SHOULD: Error logs should include operation-specific metadata such as retry attempts, timeout values, and external endpoint identifiers.

### Verify

```bash
# Discover the project's dependency manifest and identify the structured logging library
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate lock file and resolve exact version in use
find . -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' | head -1

# Search codebase for error logging patterns
grep -r "log\.Error\|logger\.Error\|slog\.Error" --include="*.go" --include="*.js" --include="*.py" | head -20

# Verify error extraction uses structured methods, not string formatting
grep -r "fmt\.Sprintf.*err\|errors\.Wrap" --include="*.go" | wc -l

# Identify test suites validating context propagation
find . -name '*test*.go' -o -name '*.test.js' | xargs grep -l "context\|correlation" | head -10

# Execute tests to confirm correlation identifiers are preserved
go test -v ./... -run TestContext 2>&1 | grep -E "PASS|FAIL|correlation"
```

**Accept when:**
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure
- Error metadata (retry attempts, timeout values, endpoint identifiers) is present in structured log fields
- No error logging uses string formatting (fmt.Sprintf, string concatenation) instead of structured field extraction

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging patterns must be inspected for compliance with R-ERRLOG-001 before accepting changes to error handling code.
</enforcement>