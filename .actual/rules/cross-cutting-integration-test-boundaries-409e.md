# Standardize Public API Contract Definitions for HTTP Testing: Integration Test Boundaries

These rules are ALWAYS ACTIVE for all HTTP checker handlers, integration test code, protocol buffer service definitions, ping and health check endpoints, and client wrapper code that interacts with external HTTP APIs.

### Rules

- **R-HTTP-001** MUST: All integration test boundaries that interact with external HTTP services MUST use the standardized contract types rather than raw HTTP client types.
- **R-HTTP-002** MUST: Define contract types in a shared package that all checker handlers import, ensuring Body, Header, Timeout, and assertion fields are consistently structured.
- **R-HTTP-003** MUST: Implement assertion evaluator functions that accept contract types rather than raw HTTP responses, separating number and string comparison logic.
- **R-HTTP-004** MUST: Ensure protocol buffer message types implement or can be adapted to the standardized HTTP contract interfaces for cross-boundary communication.
- **R-HTTP-005** MUST: Preserve access to underlying HTTP request and response objects within contract types for inspection and debugging.

### Verify

```bash
# Discover the project's test execution script and run integration tests for HTTP checker handlers to verify contract compliance
./scripts/test integration --filter=http-checker-handlers

# Locate the project's static analysis configuration and execute type checking to confirm all handlers use standardized contract types
./scripts/lint --type-check --scope=handlers

# Identify the project's assertion validation test suite and execute it to verify separation of number and string comparator functions
./scripts/test assertion-validators
```

**Accept when:**
- All HTTP checker handlers use standardized contract types for request and response structures
- Integration tests pass with consistent assertion evaluation across all handler implementations
- Static analysis confirms no direct use of raw HTTP client types in handler validation logic
- Protocol buffer message types satisfy HTTP contract interfaces
- Assertion evaluator functions are separated by type (number and string comparators)

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be checked before code is committed.
</enforcement>