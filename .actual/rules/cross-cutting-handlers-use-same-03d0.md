# Standardize Service Handler Context Extraction for Event Retrieval: Handlers Use Same

These rules are ALWAYS ACTIVE for all protocol handler implementations (HTTP, TCP, DNS) that extract event context from request objects within the checker application.

### Rules

- **R-HANDLER-001** MUST: Handlers MUST use the same context key identifier across all protocol implementations to ensure consistent event retrieval.
- **R-HANDLER-002** MUST: Define a package-level constant for the event context key to prevent string literal duplication and enable compile-time reference checking across handler implementations.
- **R-HANDLER-003** MUST: Implement explicit nil checks immediately after context extraction and return structured error responses when event data is missing, rather than allowing nil pointer dereferences in downstream logic.
- **R-HANDLER-004** SHOULD: Consider wrapping the context extraction pattern in a helper function that returns typed event data and error values, providing a single point of maintenance for context retrieval logic.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'package.json' -o -name 'pom.xml' -o -name 'Gemfile' \) | head -1

# Locate and execute the project's test suite to verify handler implementations
grep -r "func Test" . --include="*.go" | grep -i handler | head -5

# Discover the project's static analysis configuration and execute linting
find . -maxdepth 2 -type f \( -name '.golangci.yml' -o -name '.eslintrc' -o -name 'pylintrc' \) | head -1

# Search for context key usage across handler files
grep -r "context\." . --include="*.go" | grep -E "(checker|tcp|dns)\.go" | grep -i key

# Verify consistent context key identifier across all protocol handlers
grep -r "context\.WithValue\|context\.Value" . --include="*.go" | grep -E "(checker|tcp|dns)\.go"

# Execute integration tests for protocol-specific handlers
find . -type f -name "*_test.go" | xargs grep -l "handler\|Handler" | head -5
```

**Accept when:**
- All handler implementations successfully extract event context using the standardized accessor method without runtime panics or nil pointer errors.
- Static analysis confirms consistent use of the same context key identifier across all protocol handler implementations (HTTP, TCP, DNS).
- Integration tests demonstrate successful event context propagation through the complete request lifecycle for all protocol handlers.
- A package-level constant is defined for the context key and used consistently across all handler files.
- Explicit nil checks are present after context extraction with structured error responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All handler implementations MUST conform to the standardized context extraction pattern before code review approval.
</enforcement>