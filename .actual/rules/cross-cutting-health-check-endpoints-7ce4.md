# Standardize Structured Logging at Service Boundaries with Context Propagation: Health Check Endpoints

These rules are ALWAYS ACTIVE for all HTTP handler implementations, TCP and DNS protocol handlers, client initialization code, event tracking middleware, and error paths in backoff retry operations that cross service boundaries.

### Rules

- **R-SLOG-001** MUST: Implement middleware that injects correlation identifiers into request context at the earliest entry point and ensures context is passed to all downstream logging calls.
- **R-SLOG-002** MUST: Use log.Ctx(ctx).Error() with structured error messages via err.Error() for all error logging at service boundaries.
- **R-SLOG-003** MUST: Establish a standard set of HTTP headers for external client requests that includes service identification and request correlation tokens.
- **R-SLOG-004** MUST: Create structured error types that capture both the error message and contextual metadata required for service boundary diagnostics.
- **R-SLOG-005** MAY: Health check endpoints MAY include region and provider metadata in responses to support distributed system topology visibility.
- **R-SLOG-006** SHOULD: Implement log sanitization middleware that redacts authorization tokens and sensitive query parameters before emission.
- **R-SLOG-007** SHOULD: Implement sampling strategies for successful requests while maintaining full logging for error conditions and configurable log levels per handler.

### Verify

```bash
# Discover the project's dependency manifest and identify the logging library
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'requirements.txt' | head -1

# Locate verification scripts that validate context propagation through handler chains
find . -path './.actual' -prune -o -type f -name '*test*.go' -o -name '*_test.go' | grep -i 'handler\|context\|log' | head -10

# Identify test suites that exercise service boundary error conditions
grep -r 'log.Ctx\|context.Context' . --include='*_test.go' --include='*test*.go' | head -20

# Locate static analysis or linting configuration
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -5

# Verify error log entries include correlation identifiers
grep -r 'log.Ctx(ctx).Error' . --include='*.go' | wc -l

# Verify HTTP client requests include identifying headers
grep -r 'User-Agent\|Authorization\|Content-Type' . --include='*.go' | grep -i 'header\|request' | head -10
```

**Accept when:**
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts
- Middleware validates context presence and injects fallback correlation identifiers when context is missing
- Log sanitization middleware redacts authorization tokens and sensitive query parameters before emission
- Sampling strategies are implemented for successful requests while maintaining full logging for error conditions

<enforcement>
Clause MUST NOT skip or defer verification. Code review checklist MUST require context-aware logging for all new service boundary implementations. Static analysis rules MUST detect error logging without context propagation. Integration test coverage MUST include service boundary error paths with log assertions. Pull requests introducing service boundary code without context-aware logging MUST be blocked until corrected.
</enforcement>