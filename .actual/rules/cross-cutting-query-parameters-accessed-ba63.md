# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Query Parameters Accessed

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication.

### Rules

- **R-QP-001** SHOULD: Query parameters SHOULD be accessed through framework-provided accessor methods rather than raw URL parsing.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|client|handler|integration)' | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r 'query.*param\|QueryParam\|getQuery' --include='*.go' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -20

# Identify HTTP client implementations and verify they use framework accessors
grep -r 'http\..*Client\|NewClient\|Request' --include='*.go' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -20
```

**Accept when:**
- All HTTP client implementations access query parameters through framework-provided methods (e.g., `req.URL.Query()`, `request.query`, `request.args`)
- No raw URL string parsing or manual query string splitting is present in HTTP handler or client code
- Integration tests pass demonstrating correct query parameter access across service boundaries
- Code review checklist items confirm query parameter access follows framework conventions

<enforcement>
Claude Code MUST NOT skip or defer verification of query parameter access patterns. All HTTP boundary code MUST use framework-provided accessor methods.
</enforcement>