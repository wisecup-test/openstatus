# Standardize Structured Logging at Service Boundaries with Context Propagation: Error Log Entries

These rules are ALWAYS ACTIVE for all HTTP handler implementations, TCP and DNS protocol handlers, client initialization code, event tracking middleware, and error paths in backoff retry operations that cross service boundaries.

### Rules

- **R-SLOG-001** MUST: Error log entries MUST include the error message extracted via err.Error() to preserve stack trace and diagnostic context.

### Verify

```bash
# Discover the project's dependency manifest and identify the logging library
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'requirements.txt' | head -1

# Locate verification scripts that validate context propagation through handler chains
find . -path './.actual' -prune -o -type f -name '*test*.go' -print | xargs grep -l 'log.Ctx\|context' | head -5

# Identify test suites that exercise service boundary error conditions
find . -path './.actual' -prune -o -type f -name '*_test.go' -print | xargs grep -l 'Error\|error' | head -5

# Locate static analysis or linting configuration
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -1

# Verify error logging includes context propagation in handler implementations
grep -r 'log.Ctx(ctx).Error' . --include='*.go' | grep -v '.actual' | wc -l

# Verify HTTP client requests include identifying headers
grep -r 'User-Agent\|Authorization\|Content-Type' . --include='*.go' | grep -v '.actual' | head -5
```

**Accept when:**
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts
- Code review checklist confirms context-aware logging for all new service boundary implementations
- Static analysis rules detect and flag error logging without context propagation
- Integration test coverage includes assertions on structured log output for service boundary error paths

<enforcement>
Claude Code MUST NOT skip or defer verification. All error log entries crossing service boundaries MUST be reviewed for compliance with R-SLOG-001 before code acceptance.
</enforcement>