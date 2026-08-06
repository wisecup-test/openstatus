# Standardize HTTP Header Inspection for Service Boundary Testing: Integration Test Assertions

These rules are ALWAYS ACTIVE for all integration tests that validate HTTP service boundaries, HTTP client implementations that cross service boundaries, HTTP handlers that process requests from external clients, and header-based authentication and authorization flows.

### Rules

- **R-BOUNDARY-001** SHOULD: Integration test assertions SHOULD validate both request header propagation and response header presence at service boundaries.

### Verify

```bash
# Locate the project's integration test suite and execute tests that validate HTTP service boundaries
find . -path ./node_modules -prune -o -type f -name '*integration*test*' -o -name '*test*integration*' | head -20

# Search the codebase for HTTP client instantiations and verify User-Agent header configuration
grep -r "User-Agent" --include="*.js" --include="*.ts" --include="*.go" --include="*.py" --include="*.java" | grep -i "header\|client" | head -20

# Inspect integration test implementations to confirm header validation assertions are present
grep -r "assert.*header\|expect.*header\|validate.*header" --include="*test*" --include="*spec*" | head -20

# Verify Content-Type headers are set for JSON payloads
grep -r "Content-Type.*json\|application/json" --include="*test*" --include="*client*" | head -20
```

**Accept when:**
- All integration tests that cross service boundaries include assertions validating required HTTP headers
- HTTP clients consistently set User-Agent headers identifying the calling service
- Content-Type headers are validated for requests with bodies and set explicitly for JSON payloads
- Request header propagation is verified in integration test suites
- Response header presence is asserted in integration test suites

<enforcement>
Claude Code MUST NOT skip or defer verification of header validation in integration tests crossing service boundaries. Integration test failures due to missing header assertions MUST block deployment.
</enforcement>