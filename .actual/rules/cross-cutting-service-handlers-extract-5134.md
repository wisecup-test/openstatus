# Standardize Service Handler Context Extraction for Event Retrieval: Service Handlers Extract

These rules are ALWAYS ACTIVE for all service handler implementations across HTTP, TCP, and DNS protocol handlers that process incoming requests and extract event context from request objects.

### Rules

- **R-HANDLER-001** MUST: All service handlers MUST extract event context using the context accessor method provided by the HTTP framework's context object.
- **R-HANDLER-002** MUST: Define a package-level constant for the event context key to prevent string literal duplication and enable compile-time reference checking across handler implementations.
- **R-HANDLER-003** MUST: Implement explicit nil checks immediately after context extraction and return structured error responses when event data is missing, rather than allowing nil pointer dereferences in downstream logic.
- **R-HANDLER-004** SHOULD: Consider wrapping the context extraction pattern in a helper function that returns typed event data and error values, providing a single point of maintenance for context retrieval logic.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'package.json' -o -name 'pom.xml' -o -name 'Gemfile' \) | head -1

# Locate and execute the project's test suite to verify handler implementations
# (command varies by build tool; consult project's build configuration)

# Discover the project's static analysis configuration and execute linting
# (command varies by language; e.g., 'go vet', 'eslint', 'pylint')

# Execute protocol-specific handler tests to verify event context extraction behavior
# (command varies by test framework; consult project's test configuration)

# Verify consistent context key usage across handler files
grep -r "context\..*Event\|EventContext" --include="*.go" --include="*.ts" --include="*.js" | grep -E "(checker|tcp|dns)\." | sort | uniq -c
```

**Accept when:**
- All handler implementations successfully extract event context using the standardized accessor method without runtime panics or nil pointer errors
- Static analysis confirms consistent use of the same context key identifier across all protocol handler implementations (HTTP, TCP, DNS)
- Integration tests demonstrate successful event context propagation through the complete request lifecycle for all protocol handlers
- Package-level constant for context key is defined and used consistently across all handler files
- Explicit nil checks are present immediately after context extraction in all handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. All handler implementations MUST conform to R-HANDLER-001 and R-HANDLER-003 before code review approval. Static analysis verification of R-HANDLER-002 is mandatory. Violations block pull requests until conformance is achieved.
</enforcement>