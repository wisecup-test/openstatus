# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Base64 Encoding Operations

These rules are ALWAYS ACTIVE for all HTTP checker handlers, job implementations, integration test request/response processing, external HTTP client configuration, request body serialization, response body deserialization, and header manipulation and validation logic.

### Rules

- **R-STDLIB-BASE64-001** MAY: Base64 encoding operations for authentication or payload encoding may use standard library encoding packages.
- **R-STDLIB-BASE64-002** MUST: Centralize HTTP client construction with explicit timeout configuration to ensure consistent behavior across all integration test scenarios and production handlers.
- **R-STDLIB-BASE64-003** SHOULD: Establish shared utilities for common patterns including header manipulation, body serialization, and error wrapping to reduce code duplication across checker components.
- **R-STDLIB-BASE64-004** SHOULD: Document standard library package import conventions and maintain consistency in error handling patterns across all HTTP testing infrastructure.
- **R-STDLIB-BASE64-005** MUST: Use standard library packages (bytes, context, encoding/json, net/http, time, crypto/tls, errors, fmt) for HTTP testing infrastructure rather than third-party HTTP testing frameworks.
- **R-STDLIB-BASE64-006** MUST: Process request and response bodies using standard library JSON encoding/decoding without third-party serialization dependencies.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
find . -name 'go.mod' -o -name 'go.sum' | head -1
grep -r 'import' --include='*_test.go' --include='*handler*.go' --include='*client*.go' | grep -E '(bytes|context|encoding/json|net/http|time|crypto/tls|errors|fmt)' | wc -l

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
find . -name '*_test.go' -path '*/integration/*' -o -name '*integration*test*.go' | head -5

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.golintci.yml' | head -1

# Verify HTTP client construction patterns show explicit timeout configuration
grep -r 'Timeout' --include='*client*.go' --include='*handler*.go' | grep -E '(time\.|Duration)' | wc -l

# Confirm absence of third-party HTTP testing frameworks in core testing paths
grep -r 'import' --include='*_test.go' | grep -v 'encoding/json\|net/http\|bytes\|context\|time\|crypto/tls\|errors\|fmt' | grep -E '(testify|gock|httptest|httpmock)' | wc -l
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies
- Static analysis confirms no third-party HTTP testing frameworks are imported in integration test infrastructure
- Shared utilities for header manipulation, body serialization, and error wrapping are centralized and reused across checker components
- Standard library package import conventions are documented and consistently applied across all HTTP testing infrastructure

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review acceptance. Violations require architectural justification and engineering team lead approval via exception process documented in the source ADR.
</enforcement>