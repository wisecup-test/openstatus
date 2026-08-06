# Standardize Structured Error Logging with Context Propagation: Error Logs Include

These rules are ALWAYS ACTIVE for all handler functions that process HTTP, DNS, TCP, or ping monitoring requests; client interaction code that performs external HTTP requests or network operations; retry logic implementations using backoff strategies; public API contract implementations that expose error states; and integration points with third-party APIs where errors must be captured.

### Rules

- **R-LOG-001** MAY: Error logs MAY include additional structured fields such as region, provider, monitor type, or assertion results when relevant to the operational context.

### Verify

```bash
# Discover the project's dependency manifest and lock file, then locate the verification script that validates structured logging patterns across handler implementations
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' | head -1

# Identify the project's static analysis or linting configuration and execute the rule that checks for context-aware error logging method usage
find . -name '.golangci.yml' -o -name 'eslintrc*' -o -name '.lint*' | head -1

# Locate the integration test suite for protocol handlers and run tests that verify error log output includes structured fields and context correlation
find . -path '*/test*' -name '*handler*test*' -o -path '*/test*' -name '*integration*test*' | head -5
```

**Accept when:**
- All error logging call sites in handler functions use context-aware structured logging methods with error field extraction
- Static analysis confirms no error logging uses string concatenation or unstructured output methods
- Integration tests verify that error logs from retry operations include correlation identifiers and can be queried by structured fields

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis rules in continuous integration MUST detect unstructured error logging patterns. Code review MUST verify context propagation in new handler implementations. Integration test coverage MUST validate error logging paths in protocol handlers.
</enforcement>