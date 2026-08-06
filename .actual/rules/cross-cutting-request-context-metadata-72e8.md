# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Request Context Metadata

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication.

### Rules

- **R-REQCTX-001** MUST: Request context metadata MUST be propagated through framework context storage mechanisms when crossing handler boundaries.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|client|handler|integration)' | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r "User-Agent\|Content-Type\|Authorization" --include="*.go" --include="*.js" --include="*.ts" | wc -l

# Identify and execute end-to-end tests that verify header propagation across multi-service request flows
find . -type f -name '*e2e*' -o -name '*integration*' | head -10
```

**Accept when:**
- All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies
- Authorization headers are present in all external service client requests and use bearer token format
- Integration tests pass demonstrating correct header propagation and query parameter access across service boundaries
- Request context metadata is consistently propagated through framework context storage mechanisms at all handler boundaries

<enforcement>
Claude Code MUST NOT skip or defer verification of R-REQCTX-001. Integration test suites validating header contracts between services are mandatory. Code review must confirm header operations are present before approval. Runtime monitoring must alert on missing User-Agent or Authorization headers at service boundaries.
</enforcement>