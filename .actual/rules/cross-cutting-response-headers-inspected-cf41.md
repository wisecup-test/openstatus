# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Response Headers Inspected

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication.

### Rules

- **R-SVC-BND-001** SHOULD: Response headers SHOULD be inspected to extract metadata required for downstream processing or observability.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|client|handler|integration)' | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r "header" . --include="*.md" --include="*.txt" | grep -i "convention\|standard\|rule" | head -10

# Identify and execute end-to-end tests that verify header propagation across multi-service request flows
find . -type f -name '*e2e*' -o -name '*integration*' | head -10
```

**Accept when:**
- All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies
- Authorization headers are present in all external service client requests and use bearer token format
- Integration tests pass demonstrating correct header propagation and query parameter access across service boundaries
- Response headers are consistently inspected and extracted for metadata required by downstream processing or observability

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test suites validating header contracts between services are mandatory. Code review must identify missing or malformed header operations. Static analysis rules detecting missing header operations must pass before merge.
</enforcement>