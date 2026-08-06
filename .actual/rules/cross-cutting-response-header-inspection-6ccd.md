# Standardize Structured Logging at Service Boundaries with Context Propagation: Response Header Inspection

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS handler implementations that interact with external systems and cross service boundaries, including client initialization code, event tracking middleware, and error paths in retry operations.

### Rules

- **R-SVCBOUND-001** SHOULD: Response header inspection SHOULD be logged when content negotiation or protocol-specific metadata affects request processing.

### Verify

```bash
# Discover the project's dependency manifest and identify the logging library
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'requirements.txt' | head -1

# Locate verification scripts that validate context propagation through handler chains
find . -path './.actual' -prune -o -type f -name '*test*.sh' -print | grep -i context

# Identify test suites that exercise service boundary error conditions
find . -path './.actual' -prune -o -type f \( -name '*_test.go' -o -name '*_test.js' -o -name 'test_*.py' \) -print | xargs grep -l 'service.*boundary\|handler.*error' 2>/dev/null | head -5

# Confirm structured log output includes correlation identifiers
grep -r 'log\.Ctx\|context.*log\|correlation' . --include='*.go' --include='*.js' --include='*.py' 2>/dev/null | grep -v '.actual' | head -10

# Verify HTTP client requests include identifying headers
grep -r 'User-Agent\|Authorization\|Content-Type' . --include='*.go' --include='*.js' --include='*.py' 2>/dev/null | grep -v '.actual' | head -10
```

**Accept when:**
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts
- Response header inspection is logged in HTTP handler implementations that invoke external services
- TCP and DNS protocol handlers with retry logic include structured logging for header metadata

<enforcement>
Claude Code MUST NOT skip or defer verification. All service boundary implementations MUST include context-aware logging with correlation identifiers before code review approval.
</enforcement>