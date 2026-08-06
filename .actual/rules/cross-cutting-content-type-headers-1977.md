# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: Content Type Headers

These rules are ALWAYS ACTIVE for all HTTP checker handlers, TCP checker handlers, DNS checker handlers, external client wrappers, and request context caching mechanisms that interact with service boundaries through header and query parameter inspection.

### Rules

- **R-BOUNDARY-001** SHOULD: Content-Type headers SHOULD be inspected before processing request bodies and set explicitly when the content type differs from defaults.

### Verify

```bash
# Discover and execute the project's test suite focusing on handler integration tests
# that verify request context caching and header inspection
find . -type f -name '*test*' -o -name '*_test.*' | grep -E '(handler|integration)' | head -20

# Locate and run static analysis tooling configured in the repository
# to detect inconsistent cache key usage across handler implementations
ls -la | grep -E '(lint|analysis|static)'

# Identify and invoke the project's linting configuration
# to verify header accessor method usage follows framework conventions
grep -r 'Content-Type' --include='*.go' --include='*.py' --include='*.js' --include='*.ts' | head -20
```

**Accept when:**
- All handler types successfully retrieve event data from request cache using consistent key names
- External client requests include User-Agent and Authorization headers in the expected format
- Integration tests pass for all handler types demonstrating correct header and query parameter inspection
- Content-Type headers are explicitly set before processing request bodies across all handler implementations
- No direct HTTP object manipulation bypasses framework accessor methods for request metadata

<enforcement>
Clause Code MUST NOT skip or defer verification. Integration tests covering all handler types and external client interactions are mandatory. Code review must enforce header and query parameter access patterns. Static analysis must detect violations of framework accessor method usage.
</enforcement>