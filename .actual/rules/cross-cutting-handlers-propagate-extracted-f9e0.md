# Standardize Service Handler Context Extraction for Event Retrieval: Handlers Propagate Extracted

These rules are ALWAYS ACTIVE for all protocol handler implementations (HTTP, TCP, DNS) within the checker application that extract event context from request objects and perform health checks against external services.

### Rules

- **R-HANDLER-001** SHOULD: Handlers SHOULD propagate the extracted context to logging operations for observability correlation.
- **R-HANDLER-002** MUST: Define a package-level constant for the event context key to prevent string literal duplication and enable compile-time reference checking across handler implementations.
- **R-HANDLER-003** MUST: Implement explicit nil checks immediately after context extraction and return structured error responses when event data is missing, rather than allowing nil pointer dereferences in downstream logic.
- **R-HANDLER-004** SHOULD: Consider wrapping the context extraction pattern in a helper function that returns typed event data and error values, providing a single point of maintenance for context retrieval logic.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool, then locate and execute the project's test suite to verify handler implementations
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' | head -1

# Discover the project's static analysis configuration and execute linting or type-checking tools to verify context key consistency across handler files
grep -r "context\.WithValue\|context\.Value" --include="*.go" checker.go tcp.go dns.go 2>/dev/null || echo "Context extraction pattern not found"

# Discover the project's integration test infrastructure and execute protocol-specific handler tests to verify event context extraction behavior
find . -name '*_test.go' -path '*/handler*' -o -name '*_test.go' -path '*/checker*' | head -5
```

**Accept when:**
- All handler implementations successfully extract event context using the standardized accessor method without runtime panics or nil pointer errors.
- Static analysis confirms consistent use of the same context key identifier across all protocol handler implementations (HTTP, TCP, DNS).
- Integration tests demonstrate successful event context propagation through the complete request lifecycle for HTTP, TCP, and DNS handlers.
- A package-level constant for the context key is defined and used consistently across all handler files.
- Explicit nil checks are present immediately after context extraction with structured error responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All handler implementations MUST conform to the standardized context extraction pattern before code is committed.
</enforcement>