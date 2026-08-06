# Standardize HTTP Header Manipulation for Testing Strategy: Custom Headers Iterable

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, request and response validation logic, and client configuration for outbound HTTP requests.

### Rules

- **R-HTTP-HDR-001** SHOULD: Custom headers SHOULD be iterable and applied to requests through a loop or batch operation to support dynamic header configuration.
- **R-HTTP-HDR-002** SHOULD: Identify the standard library HTTP client interface used in the codebase and ensure all checker components use consistent methods for header manipulation.
- **R-HTTP-HDR-003** SHOULD: Establish shared constants or configuration for standard headers to avoid string literal duplication across test implementations.
- **R-HTTP-HDR-004** SHOULD: Document the expected header contracts for each external service being validated, including required headers, optional headers, and validation criteria.
- **R-HTTP-HDR-005** SHOULD: Implement centralized error handling for HTTP client operations that distinguishes between network errors, timeout errors, and application-level failures.

### Verify

```bash
# Locate and run the project's integration test suite
find . -name '*test*' -type f | grep -E '(integration|http|checker)' | head -5

# Inspect test output logs to confirm request headers are being set
grep -r 'header' . --include='*test*.py' --include='*test*.go' --include='*test*.js' | head -10

# Search for header manipulation method invocations across checker components
grep -r 'set.*header\|add.*header\|Header' . --include='*.py' --include='*.go' --include='*.js' | grep -v node_modules | head -15

# Verify consistent usage patterns in HTTP client operations
grep -r 'requests\|http\.Client\|fetch' . --include='*test*.py' --include='*test*.go' --include='*test*.js' | wc -l
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms consistent use of header manipulation interfaces across all HTTP checker components
- Test logs demonstrate proper header values are set on outbound requests and validated on inbound responses
- Shared header constants or helper functions are identified and documented
- External service header contracts are documented with required, optional, and validation criteria

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test failures due to incorrect header configuration block merge requests. Code review feedback requires correction of non-standard header manipulation patterns. Static analysis warnings are escalated to errors for critical header operations.
</enforcement>