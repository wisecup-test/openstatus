# Standardize Structured Logging at Service Boundaries with Context Propagation: Error Conditions Service

These rules are ALWAYS ACTIVE for all HTTP handler implementations, TCP and DNS protocol handlers, client initialization code, event tracking middleware, and error paths in backoff retry operations that cross service boundaries.

### Rules

- **R-SLOG-001** MUST: All error conditions at service boundaries MUST be logged using context-aware structured logging that propagates request correlation identifiers.

### Verify

```bash
# Discover the project's dependency manifest and identify the logging library
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'requirements.txt' | head -1

# Locate verification scripts that validate context propagation through handler chains
find . -path './.actual' -prune -o -type f -name '*test*.go' -print | xargs grep -l 'log.Ctx\|context.*propagat' 2>/dev/null | head -5

# Identify test suites that exercise service boundary error conditions
find . -path './.actual' -prune -o -type f -name '*_test.go' -print | xargs grep -l 'Error\|error.*log' 2>/dev/null | head -5

# Locate static analysis or linting configuration
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' 2>/dev/null

# Verify context-aware logging pattern in handler implementations
grep -r 'log.Ctx(ctx).Error' . --include='*.go' 2>/dev/null | wc -l

# Check for HTTP client header injection patterns
grep -r 'User-Agent\|Authorization\|Content-Type' . --include='*.go' 2>/dev/null | grep -i 'header\|set' | head -5
```

**Accept when:**
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts
- Static analysis rules detect and flag error logging without context propagation
- Integration test coverage includes service boundary error paths with log assertions

<enforcement>
Claude Code MUST NOT skip or defer verification. All service boundary error logging MUST use context-aware structured logging with correlation identifiers before code is accepted.
</enforcement>