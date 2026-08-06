# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: Authorization Headers Constructed

These rules are ALWAYS ACTIVE for all HTTP checker handlers, TCP checker handlers, DNS checker handlers, external client wrappers, and request context caching mechanisms that interact with service boundaries and external datastores.

### Rules

- **R-BOUNDARY-001** MUST: Authorization headers MUST be constructed using bearer token formatting when communicating with external datastores.

### Verify

```bash
# Discover and execute the project's test suite focusing on handler integration tests
# that verify request context caching and header inspection
find . -type f -name '*test*' -o -name '*spec*' | grep -i handler | head -20

# Locate and run static analysis tooling configured in the repository to detect
# inconsistent cache key usage across handler implementations
grep -r "cache.*key" --include="*.go" --include="*.py" --include="*.js" --include="*.ts" | wc -l

# Identify and invoke the project's linting configuration to verify header accessor
# method usage follows framework conventions
grep -r "Authorization" --include="*.go" --include="*.py" --include="*.js" --include="*.ts" | grep -i bearer
```

**Accept when:**
- All handler types successfully retrieve event data from request cache using consistent key names
- External client requests include User-Agent and Authorization headers in the expected bearer token format
- Integration tests pass for all handler types demonstrating correct header and query parameter inspection
- Static analysis detects no inconsistent cache key naming across handler implementations
- Code review confirms no direct HTTP object manipulation bypasses framework accessor methods

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration tests covering all handler types and external client interactions are mandatory before accepting any implementation. Authorization header construction MUST follow bearer token formatting without exception.
</enforcement>