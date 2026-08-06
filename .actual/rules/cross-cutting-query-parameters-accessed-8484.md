# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: Query Parameters Accessed

These rules are ALWAYS ACTIVE for all HTTP checker handlers, TCP checker handlers, DNS checker handlers, external client wrappers, and request context caching mechanisms that interact with service boundaries through header and query parameter inspection.

### Rules

- **R-BOUNDARY-001** MUST: Query parameters MUST be accessed through the framework's query accessor methods rather than direct URL parsing.

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
- Static analysis detects no instances of direct URL parsing for query parameter extraction
- Code review confirms all query parameter access uses framework accessor methods

<enforcement>
Clause R-BOUNDARY-001 verification is mandatory. Integration tests MUST pass before merge. Code review MUST confirm framework accessor method usage. Static analysis MUST detect no violations of direct URL parsing patterns.
</enforcement>