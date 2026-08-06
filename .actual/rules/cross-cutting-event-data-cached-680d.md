# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: Event Data Cached

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS checker handlers, external client wrappers, and request context caching mechanisms that interact with service boundaries through header and query parameter inspection.

### Rules

- **R-BOUNDARY-001** MUST: Event data cached in the request context MUST be set with consistent key names across all handler types (HTTP, TCP, DNS).

### Verify

```bash
# Discover and execute the project's test suite focusing on handler integration tests
# that verify request context caching and header inspection
find . -type f -name '*test*' -o -name '*_test.*' | grep -i handler | head -20

# Locate and run static analysis tooling configured in the repository to detect
# inconsistent cache key usage across handler implementations
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' | head -5

# Identify and invoke the project's linting configuration to verify header accessor
# method usage follows framework conventions
grep -r "cache.*key\|CACHE.*KEY" --include="*.go" --include="*.py" --include="*.js" --include="*.ts" | grep -E "const|var|let|=" | sort -u
```

**Accept when:**
- All handler types successfully retrieve event data from request cache using consistent key names
- External client requests include User-Agent and Authorization headers in the expected format
- Integration tests pass for all handler types demonstrating correct header and query parameter inspection
- Static analysis detects no inconsistent cache key naming across handler implementations
- Code review confirms all handlers use framework accessor methods rather than direct HTTP object manipulation

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration tests covering all handler types and external client interactions are mandatory before accepting changes. Code review checklist items for header and query parameter access patterns must be satisfied. Static analysis rules detecting direct HTTP object manipulation must pass.
</enforcement>