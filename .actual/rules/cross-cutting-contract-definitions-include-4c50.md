# Standardize Public API Contract Definitions for HTTP Testing: Contract Definitions Include

These rules are ALWAYS ACTIVE for all HTTP checker handlers, integration test code, protocol buffer service definitions, ping and health check endpoints, and client wrapper code that interacts with external HTTP APIs.

### Rules

- **R-CONTRACT-001** SHOULD: Contract definitions SHOULD include timing metadata structures to capture latency and performance metrics.
- **R-CONTRACT-002** MUST: Define contract types in a shared package that all checker handlers import, ensuring Body, Header, Timeout, and assertion fields are consistently structured.
- **R-CONTRACT-003** MUST: Implement assertion evaluator functions that accept contract types rather than raw HTTP responses, separating number and string comparison logic.
- **R-CONTRACT-004** MUST: Ensure protocol buffer message types implement or can be adapted to the standardized HTTP contract interfaces for cross-boundary communication.
- **R-CONTRACT-005** MUST: Avoid direct use of raw HTTP client types in handler validation logic; use standardized contract types instead.

### Verify

```bash
# Discover the project's test execution script and run integration tests for HTTP checker handlers
# to verify contract compliance
find . -name "*test*" -o -name "*integration*" | head -5

# Locate the project's static analysis configuration and execute type checking
# to confirm all handlers use standardized contract types
find . -name ".golangci.yml" -o -name "go.mod" -o -name "*.proto" | head -5

# Identify the project's assertion validation test suite and execute it
# to verify separation of number and string comparator functions
grep -r "ProtoNumberAssertionToComparator\|ProtoStringAssertionToComparator" . --include="*.go" | head -10
```

**Accept when:**
- All HTTP checker handlers use standardized contract types for request and response structures
- Integration tests pass with consistent assertion evaluation across all handler implementations
- Static analysis confirms no direct use of raw HTTP client types in handler validation logic
- Protocol buffer message types satisfy HTTP contract interfaces
- Timing metadata structures are present in contract definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and continuous integration validation.
</enforcement>