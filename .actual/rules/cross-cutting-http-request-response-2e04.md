# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Http Request Response

These rules are ALWAYS ACTIVE for all HTTP request and response processing in integration test paths, HTTP checker handlers, job implementations, external HTTP client configuration, request body serialization, response body deserialization, and header manipulation and validation logic.

### Rules

- **R-HTTP-001** MUST: All HTTP request and response processing in integration test paths must use standard library packages for encoding, context propagation, and network operations.
- **R-HTTP-002** MUST: Centralize HTTP client construction with explicit timeout configuration to ensure consistent behavior across all integration test scenarios and production handlers.
- **R-HTTP-003** MUST: Establish shared utilities for common patterns including header manipulation, body serialization, and error wrapping to reduce code duplication across checker components.
- **R-HTTP-004** MUST: Document standard library package import conventions and maintain consistency in error handling patterns across all HTTP testing infrastructure.
- **R-HTTP-005** SHOULD: Extract common retry patterns into shared utilities and enforce usage through code review and static analysis.
- **R-HTTP-006** SHOULD: Isolate HTTP client construction behind factory functions to enable future implementation swapping without widespread code changes.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
find . -name 'go.mod' -o -name 'go.sum' | head -1
grep -r 'import.*\(\|import' --include='*_test.go' --include='*handler*.go' --include='*client*.go' | grep -E '(bytes|context|encoding/json|net/http|time|crypto/tls|errors|fmt)' | wc -l

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
find . -name '*_test.go' -path '*/integration/*' -o -name '*integration*test*.go' | head -5

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name '.golangci.yml' -o -name 'golangci.yml' -o -name '.golintci.yaml' | head -1
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies
- No third-party HTTP testing frameworks are present in core testing paths
- Shared utilities exist for header manipulation, body serialization, and error wrapping
- HTTP client construction is centralized behind factory functions or utility functions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review acceptance.
</enforcement>