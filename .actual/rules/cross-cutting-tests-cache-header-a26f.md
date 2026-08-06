# Standardize HTTP Header Inspection for Service Boundary Testing: Tests Cache Header

These rules are ALWAYS ACTIVE for all HTTP client implementations that cross service boundaries, integration tests that validate external service contracts, HTTP handlers that process requests from external clients, and header-based authentication and authorization flows.

### Rules

- **R-CACHE-001** MAY: Tests MAY cache header values in request context for validation across multiple service calls.

### Verify

```bash
# Locate the project's integration test suite and execute tests that validate HTTP service boundaries
find . -path ./node_modules -prune -o -type f -name '*test*' -o -name '*spec*' | grep -E '(test|spec)\.(js|ts|go|py)$' | head -20

# Search the codebase for HTTP client instantiations and verify User-Agent header configuration
grep -r "User-Agent" --include="*.js" --include="*.ts" --include="*.go" --include="*.py" | grep -v node_modules | head -20

# Inspect integration test implementations to confirm header validation assertions are present
grep -r "header" --include="*test*" --include="*spec*" | grep -E "(assert|expect|validate)" | head -20
```

**Accept when:**
- All integration tests that cross service boundaries include assertions validating required HTTP headers
- HTTP clients consistently set User-Agent headers identifying the calling service
- Content-Type headers are validated for requests with bodies and set explicitly for JSON payloads
- Header values are cached in request context where validation across multiple service calls is required

<enforcement>
Clause Code MUST NOT skip or defer verification. Integration test suite execution in continuous integration pipeline, code review verification of HTTP client User-Agent configuration, and static analysis detection of HTTP client instantiations without header configuration are mandatory.
</enforcement>