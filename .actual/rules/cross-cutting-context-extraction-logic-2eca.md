# Standardize Service Handler Context Extraction for Event Retrieval: Context Extraction Logic

These rules are ALWAYS ACTIVE for all protocol handler implementations (HTTP, TCP, DNS) that extract event context from request objects within the checker application.

### Rules

- **R-CTXEXT-001** SHOULD: Context extraction logic SHOULD be isolated from retry and resilience mechanisms to maintain separation of concerns.
- **R-CTXEXT-002** MUST: Define a package-level constant for the event context key to prevent string literal duplication and enable compile-time reference checking across handler implementations.
- **R-CTXEXT-003** MUST: Implement explicit nil checks immediately after context extraction and return structured error responses when event data is missing, rather than allowing nil pointer dereferences in downstream logic.
- **R-CTXEXT-004** SHOULD: Consider wrapping the context extraction pattern in a helper function that returns typed event data and error values, providing a single point of maintenance for context retrieval logic.
- **R-CTXEXT-005** MUST: Use the standardized accessor method consistently across all protocol handler implementations (checker.go, tcp.go, dns.go).

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'pom.xml' -o -name 'build.gradle' \) | head -1

# Locate and execute the project's test suite to verify handler implementations
go test -v ./... -run Handler

# Discover the project's static analysis configuration and execute linting
go vet ./...
go fmt ./...

# Search for context key usage consistency across handler files
grep -r "context\." checker.go tcp.go dns.go | grep -E "(Get|Value)" | sort | uniq -c

# Execute protocol-specific handler tests to verify event context extraction
go test -v ./... -run "TestHTTPHandler|TestTCPHandler|TestDNSHandler"
```

**Accept when:**
- All handler implementations successfully extract event context using the standardized accessor method without runtime panics or nil pointer errors.
- Static analysis confirms consistent use of the same context key identifier across all protocol handler implementations.
- Integration tests demonstrate successful event context propagation through the complete request lifecycle for HTTP, TCP, and DNS handlers.
- A package-level constant for the context key is defined and used uniformly across all handler files.
- Explicit nil checks are present immediately after context extraction in all handler implementations.
- No string literal context keys appear in handler code; all use the centralized constant.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules must be validated before accepting handler implementations that extract event context.
</enforcement>