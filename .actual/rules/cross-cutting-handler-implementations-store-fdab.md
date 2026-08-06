# Standardize Structured Logging at Service Boundaries with Context Propagation: Handler Implementations Store

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS handler implementations that interact with external systems, client initialization code that configures timeouts and transport settings, event tracking middleware that manages request correlation, and error paths in backoff retry operations.

### Rules

- **R-SSLOGS-001** SHOULD: Handler implementations SHOULD store and retrieve event correlation data using the request context storage mechanism.

### Verify

```bash
# Discover the project's dependency manifest and identify the logging library
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'requirements.txt' | head -1

# Locate verification scripts that validate context propagation through handler chains
find . -path './.actual' -prune -o -type f -name '*test*.sh' -print | grep -i context

# Identify test suites that exercise service boundary error conditions
find . -path './.actual' -prune -o -type f \( -name '*_test.go' -o -name '*_test.js' -o -name 'test_*.py' \) -print | xargs grep -l 'service.*boundary\|handler.*error' 2>/dev/null | head -5

# Locate static analysis or linting configuration that enforces context-aware logging patterns
find . -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name 'setup.cfg' | xargs grep -l 'log\|context' 2>/dev/null

# Verify error log entries include correlation identifiers
grep -r 'log\.Ctx(ctx)' . --include='*.go' 2>/dev/null | head -10

# Verify HTTP client requests include identifying headers
grep -r 'User-Agent\|Authorization\|Content-Type' . --include='*.go' 2>/dev/null | grep -i 'header\|set' | head -10
```

**Accept when:**
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts
- Context propagation middleware is present at the earliest entry point for all handler chains
- Static analysis rules or linting configuration enforces context-aware logging patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and pull request acceptance.
</enforcement>