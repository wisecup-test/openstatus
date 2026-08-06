# Standardize Structured Error Logging with Context Propagation: Handler Functions That

These rules are ALWAYS ACTIVE for all handler functions that coordinate external client operations across HTTP, DNS, TCP, and ping monitoring requests, as well as client interaction code performing external network operations, retry logic implementations, public API contract implementations, and third-party API integration points.

### Rules

- **R-LOGGING-001** MUST: Handler functions that coordinate external client operations MUST log errors with context propagated from the request lifecycle using context-aware structured logging methods.

### Verify

```bash
# Discover the project's dependency manifest and lock file
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' | head -5

# Locate the project's static analysis or linting configuration
find . -name '.golangci.yml' -o -name 'eslintrc*' -o -name '.lintrc*' | head -5

# Search for error logging patterns in handler functions
grep -r 'log\.Ctx(ctx)\.Error\|logger\.Error' --include='*.go' --include='*.ts' --include='*.js' | grep -E '(handler|Handler|client|Client)' | head -20

# Verify context propagation in handler entry points
grep -r 'func.*handler\|func.*Handler' --include='*.go' -A 5 | grep -E 'ctx|context' | head -20

# Locate integration test suite for protocol handlers
find . -path '*/test*' -name '*handler*' -o -path '*/test*' -name '*integration*' | head -10
```

**Accept when:**
- All error logging call sites in handler functions use context-aware structured logging methods with error field extraction (e.g., `log.Ctx(ctx).Error()` with `.Err(err)` field)
- Static analysis confirms no error logging uses string concatenation or unstructured output methods
- Integration tests verify that error logs from retry operations include correlation identifiers and can be queried by structured fields
- Handler entry points extract or create request correlation identifiers and attach them to context before passing to downstream functions
- Standard structured field names are consistently used across handlers for operation type, retry attempt, external endpoint, and monitor identifier

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging in handler functions MUST propagate context and use structured logging methods before code is committed.
</enforcement>