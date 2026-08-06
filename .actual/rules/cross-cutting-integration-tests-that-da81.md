# Standardize HTTP Header Inspection for Service Boundary Testing: Integration Tests That

These rules are ALWAYS ACTIVE for all integration tests that validate HTTP service boundaries, HTTP client implementations that cross service boundaries, HTTP handlers that process requests from external clients, and header-based authentication and authorization flows.

### Rules

- **R-SVC-BND-001** MUST: Integration tests that validate service boundaries MUST inspect HTTP response headers to verify contract compliance.

### Verify

```bash
# Locate the project's integration test suite and execute tests that validate HTTP service boundaries
find . -path ./node_modules -prune -o -type f -name '*integration*test*' -o -name '*test*integration*' | head -20

# Search the codebase for HTTP client instantiations and verify User-Agent header configuration
grep -r "User-Agent" --include="*.js" --include="*.ts" --include="*.go" --include="*.py" --include="*.java" . 2>/dev/null | grep -v node_modules | head -20

# Inspect integration test implementations to confirm header validation assertions are present
grep -r "header" --include="*test*" --include="*spec*" . 2>/dev/null | grep -i "assert\|expect\|validate" | head -20
```

**Accept when:**
- All integration tests that cross service boundaries include assertions validating required HTTP headers
- HTTP clients consistently set User-Agent headers identifying the calling service
- Content-Type headers are validated for requests with bodies and set explicitly for JSON payloads

<enforcement>
Clause R-SVC-BND-001 verification is mandatory. Claude Code MUST NOT skip or defer verification of header inspection in service boundary integration tests.
</enforcement>