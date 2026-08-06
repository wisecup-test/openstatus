# Standardize Public API Contract Definitions for HTTP Testing: Contract Types Include

These rules are ALWAYS ACTIVE for all HTTP checker handlers, integration test code, protocol buffer service definitions, ping and health check endpoints, and client wrapper code that interacts with external HTTP APIs.

### Rules

- **R-CONTRACT-001** MAY: Contract types MAY include protocol buffer message interfaces for cross-service communication.
- **R-CONTRACT-002** MUST: Define contract types in a shared package that all checker handlers import, ensuring Body, Header, Timeout, and assertion fields are consistently structured.
- **R-CONTRACT-003** MUST: Implement assertion evaluator functions that accept contract types rather than raw HTTP responses, separating number and string comparison logic.
- **R-CONTRACT-004** MUST: Ensure protocol buffer message types implement or can be adapted to the standardized HTTP contract interfaces for cross-boundary communication.
- **R-CONTRACT-005** MUST NOT: Use raw HTTP client types directly in handler validation logic; all validation must route through standardized contract types.

### Verify

```bash
# Discover the project's test execution script and run integration tests for HTTP checker handlers to verify contract compliance
./scripts/test.sh --integration --filter=http-checker

# Locate the project's static analysis configuration and execute type checking to confirm all handlers use standardized contract types
./scripts/lint.sh --type-check

# Identify the project's assertion validation test suite and execute it to verify separation of number and string comparator functions
./scripts/test.sh --suite=assertion-validation
```

**Accept when:**
- All HTTP checker handlers use standardized contract types for request and response structures
- Integration tests pass with consistent assertion evaluation across all handler implementations
- Static analysis confirms no direct use of raw HTTP client types in handler validation logic
- Protocol buffer message types satisfy HTTP contract interfaces
- Assertion evaluator functions are separated by type (number and string comparators)

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code touching HTTP checker handlers, integration tests, protocol buffer definitions, or HTTP client boundaries. Violations must be remediated before merge.
</enforcement>