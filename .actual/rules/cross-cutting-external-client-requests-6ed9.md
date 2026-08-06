# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: External Client Requests

These rules are ALWAYS ACTIVE for all HTTP checker handlers, TCP checker handlers, DNS checker handlers, external client wrappers, and request context caching mechanisms that interact with service boundaries through header and query parameter inspection.

### Rules

- **R-BOUNDARY-001** MUST: External client requests MUST set User-Agent headers to identify the service origin.

### Verify

```bash
# Discover and execute the project's test suite focusing on handler integration tests
# that verify request context caching and header inspection
find . -type f -name '*test*' -o -name '*spec*' | grep -i handler | head -20

# Locate and run static analysis tooling configured in the repository to detect
# inconsistent cache key usage across handler implementations
grep -r "cache.*key" --include="*.go" --include="*.py" --include="*.js" --include="*.ts" | head -20

# Identify and invoke the project's linting configuration to verify header accessor
# method usage follows framework conventions
ls -la | grep -E "lint|eslint|golangci|flake8|pylint"
```

**Accept when:**
- All handler types successfully retrieve event data from request cache using consistent key names
- External client requests include User-Agent and Authorization headers in the expected format
- Integration tests pass for all handler types demonstrating correct header and query parameter inspection
- Static analysis detects no inconsistent cache key usage across handler implementations
- Code review confirms no direct HTTP object manipulation bypassing framework accessors

<enforcement>
Clause MUST NOT skip or defer verification. Integration tests covering all handler types and external client interactions are mandatory. Code review checklist items for header and query parameter access patterns are mandatory. Static analysis rules detecting direct HTTP object manipulation bypassing framework accessors are mandatory. CI pipeline MUST fail if integration tests detect missing or malformed headers in external client requests.
</enforcement>