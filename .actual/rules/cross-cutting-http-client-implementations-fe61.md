# Standardize HTTP Header Inspection for Service Boundary Testing: Http Client Implementations

These rules are ALWAYS ACTIVE for all HTTP client implementations that cross service boundaries, integration tests that validate external service contracts, HTTP handlers that process requests from external clients, and header-based authentication and authorization flows.

### Rules

- **R-HTTP-001** MUST: HTTP client implementations MUST set User-Agent headers to identify the calling service.

### Verify

```bash
# Locate the project's integration test suite and execute tests that validate HTTP service boundaries
find . -type f -name '*test*' -o -name '*spec*' | grep -E '\.(go|js|py|java|ts)$' | head -20

# Search the codebase for HTTP client instantiations and verify User-Agent header configuration
grep -r "User-Agent" --include="*.go" --include="*.js" --include="*.py" --include="*.java" --include="*.ts" .

# Inspect integration test implementations to confirm header validation assertions are present
grep -r "header" --include="*test*" --include="*spec*" . | grep -i "assert\|expect\|validate"

# Verify HTTP clients set headers explicitly
grep -r "SetHeader\|Header(\|headers\[" --include="*.go" --include="*.js" --include="*.py" --include="*.java" --include="*.ts" . | grep -v node_modules
```

**Accept when:**
- All integration tests that cross service boundaries include assertions validating required HTTP headers
- HTTP clients consistently set User-Agent headers identifying the calling service
- Content-Type headers are validated for requests with bodies and set explicitly for JSON payloads

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test suite execution in continuous integration pipeline, code review verification, and static analysis are mandatory before approval.
</enforcement>