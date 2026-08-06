# Standardize Service Handler Context Extraction for Event Retrieval: Implementations Cache Extracted

These rules are ALWAYS ACTIVE for all protocol handler implementations (HTTP, TCP, DNS) that extract event context from request objects within the checker application.

### Rules

- **R-HANDLER-001** MAY: Implementations MAY cache extracted event data in the request context using the framework's context storage mechanism.
- **R-HANDLER-002** MUST: Define a package-level constant for the event context key to prevent string literal duplication and enable compile-time reference checking across handler implementations.
- **R-HANDLER-003** MUST: Implement explicit nil checks immediately after context extraction and return structured error responses when event data is missing, rather than allowing nil pointer dereferences in downstream logic.
- **R-HANDLER-004** SHOULD: Consider wrapping the context extraction pattern in a helper function that returns typed event data and error values, providing a single point of maintenance for context retrieval logic.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool, then locate and execute the project's test suite
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'Gemfile' | head -1

# Execute the project's test suite to verify handler implementations
go test ./... -v

# Discover and execute static analysis configuration to verify context key consistency
go vet ./...

# Execute integration tests for protocol-specific handlers
go test -tags=integration ./...
```

**Accept when:**
- All handler implementations successfully extract event context using the standardized accessor method without runtime panics or nil pointer errors
- Static analysis confirms consistent use of the same context key identifier across all protocol handler implementations (checker.go, tcp.go, dns.go)
- Integration tests demonstrate successful event context propagation through the complete request lifecycle for HTTP, TCP, and DNS handlers
- Package-level context key constant is defined and used consistently across all handler files
- Explicit nil checks are present after all context extraction operations with structured error returns

<enforcement>
Clause Code MUST NOT skip or defer verification. All handler implementations MUST conform to R-HANDLER-002 and R-HANDLER-003 before merge. Static analysis and integration tests MUST pass. Code review MUST verify pattern compliance.
</enforcement>