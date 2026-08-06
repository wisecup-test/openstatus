# Standardize Structured Error Logging with Context Propagation: Public Contract Implementations

These rules are ALWAYS ACTIVE for all handler functions that process HTTP, DNS, TCP, or ping monitoring requests; client interaction code that performs external HTTP requests or network operations; retry logic implementations using backoff strategies; public API contract implementations that expose error states; and integration points with third-party APIs where errors must be captured.

### Rules

- **R-LOGGING-001** SHOULD: Public API contract implementations that return error responses SHOULD log the error with sufficient context to correlate the log entry with the response body.

### Verify

```bash
# Discover the project's dependency manifest and lock file
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' | head -5

# Locate the verification script that validates structured logging patterns across handler implementations
find . -path './.actual/rules' -prune -o -type f -name '*test*.go' -o -name '*_test.go' | grep -i handler | head -10

# Identify the project's static analysis or linting configuration
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'lint.config.*' | head -5

# Search for error logging call sites in handler functions
grep -r 'log\.Ctx(ctx)\.Error\|logger\.Error' --include='*.go' | grep -E '(handler|Handler)' | head -20

# Verify context propagation in handler entry points
grep -r 'log\.Ctx(ctx)' --include='*.go' | wc -l

# Check for unstructured error logging patterns (string concatenation)
grep -r 'log\.Error(".*" \+\|logger\.Error(".*" \+' --include='*.go' | head -10
```

**Accept when:**
- All error logging call sites in handler functions use context-aware structured logging methods with error field extraction
- Static analysis confirms no error logging uses string concatenation or unstructured output methods
- Integration tests verify that error logs from retry operations include correlation identifiers and can be queried by structured fields
- Handler entry points extract or create request correlation identifiers and attach them to the context object before passing to downstream functions
- Standard structured field names for common error contexts (operation type, retry attempt, external endpoint, monitor identifier) are consistently used across all handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging in public API contract implementations MUST propagate request context through structured logging methods before proceeding.
</enforcement>