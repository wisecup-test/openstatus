# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: Response Headers Inspected

These rules are ALWAYS ACTIVE for all HTTP checker handlers, TCP checker handlers, DNS checker handlers, external client wrappers, and request context caching mechanisms that interact with service boundaries through header and query parameter inspection.

### Rules

- **R-BOUNDARY-001** SHOULD: Response headers SHOULD be inspected by key to extract metadata required for downstream processing.

### Verify

```bash
# Discover and execute the project's test suite focusing on handler integration tests
# that verify request context caching and header inspection
find . -type f -name '*test*' -o -name '*_test.*' | grep -E '(handler|integration)' | head -20

# Locate and run static analysis tooling configured in the repository to detect
# inconsistent cache key usage across handler implementations
if [ -f '.golangci.yml' ] || [ -f 'golangci.yml' ]; then
  echo "Found Go linter config"
fi

# Identify and invoke the project's linting configuration to verify header accessor
# method usage follows framework conventions
if [ -f 'Makefile' ]; then
  grep -E '(lint|test|check)' Makefile | head -10
fi
```

**Accept when:**
- All handler types successfully retrieve event data from request cache using consistent key names
- External client requests include User-Agent and Authorization headers in the expected format
- Integration tests pass for all handler types demonstrating correct header and query parameter inspection
- Static analysis detects no inconsistent cache key usage across handler implementations
- Code review confirms no direct HTTP object manipulation bypassing framework accessors

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration tests covering all handler types and external client interactions are mandatory before accepting changes. Code review checklist items for header and query parameter access patterns must be satisfied. Static analysis rules detecting direct HTTP object manipulation must pass.
</enforcement>