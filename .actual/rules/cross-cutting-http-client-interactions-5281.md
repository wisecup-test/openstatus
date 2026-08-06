# Standardize Structured Logging at Service Boundaries with Context Propagation: Http Client Interactions

These rules are ALWAYS ACTIVE for all HTTP client interactions with external services, TCP and DNS protocol handlers with retry logic, client initialization code that configures timeouts and transport settings, event tracking middleware that manages request correlation, and error paths in backoff retry operations.

### Rules

- **R-HTTP-001** MUST: HTTP client interactions with external services MUST set identifying headers to enable request tracing across system boundaries.

### Verify

```bash
# Discover the project's dependency manifest and identify the logging library
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'requirements.txt' | head -1

# Locate verification scripts that validate context propagation through handler chains
find . -path './.actual' -prune -o -type f -name '*test*.go' -o -name '*_test.go' | grep -E '(handler|client|context)' | head -5

# Identify test suites that exercise service boundary error conditions
grep -r 'log.Ctx\|context.Context' . --include='*_test.go' | head -10

# Locate static analysis or linting configuration
find . -name '.golangci.yml' -o -name 'lint.yaml' -o -name '.eslintrc' | head -1

# Verify HTTP client requests include identifying headers
grep -r 'User-Agent\|Authorization\|Content-Type' . --include='*.go' | grep -i 'header\|request' | head -10

# Confirm structured log output includes correlation identifiers
grep -r 'log.Ctx(ctx)' . --include='*.go' | head -10
```

**Accept when:**
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts
- Static analysis rules detect and flag error logging without context propagation
- Integration test coverage exists for service boundary error paths with log assertions

<enforcement>
Claude Code MUST NOT skip or defer verification. All service boundary HTTP client code MUST be reviewed for context propagation and identifying header injection before approval.
</enforcement>