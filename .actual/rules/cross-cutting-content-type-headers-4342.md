# Standardize HTTP Header Inspection for Service Boundary Testing: Content Type Headers

These rules are ALWAYS ACTIVE for all HTTP client implementations that cross service boundaries, integration tests that validate external service contracts, HTTP handlers that process requests from external clients, and header-based authentication and authorization flows.

### Rules

- **R-BOUNDARY-001** MUST: Content-Type headers MUST be validated when request bodies are present, and MUST be set explicitly when sending JSON payloads.

### Verify

```bash
# Locate the project's integration test suite and execute tests that validate HTTP service boundaries
find . -path ./node_modules -prune -o -type f -name '*test*' -o -name '*spec*' | grep -E '(test|spec)\.(js|ts|go|py)$' | head -20

# Search the codebase for HTTP client instantiations and verify User-Agent header configuration
grep -r "Content-Type" --include="*.js" --include="*.ts" --include="*.go" --include="*.py" | grep -E '(header|Header|setHeader)' | head -20

# Inspect integration test implementations to confirm header validation assertions are present
grep -r "assert.*[Hh]eader\|expect.*[Hh]eader\|Content-Type" --include="*test*" --include="*spec*" | head -20
```

**Accept when:**
- All integration tests that cross service boundaries include assertions validating required HTTP headers
- HTTP clients consistently set Content-Type headers explicitly for JSON payloads
- Content-Type headers are validated for requests with bodies in integration test suites
- No HTTP client implementations sending request bodies lack explicit Content-Type header configuration

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test failures blocking deployment when header validation assertions fail is mandatory.
</enforcement>