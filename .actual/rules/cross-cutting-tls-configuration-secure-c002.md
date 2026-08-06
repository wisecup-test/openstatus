# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Tls Configuration Secure

These rules are ALWAYS ACTIVE for all HTTP checker handlers, job implementations, integration test request/response processing, external HTTP client configuration, request body serialization, response body deserialization, and header manipulation and validation logic.

### Rules

- **R-TLS-001** SHOULD: TLS configuration for secure HTTP testing should use standard library crypto packages.
- **R-TLS-002** MUST: Centralize HTTP client construction with explicit timeout configuration to ensure consistent behavior across all integration test scenarios and production handlers.
- **R-TLS-003** SHOULD: Establish shared utilities for common patterns including header manipulation, body serialization, and error wrapping to reduce code duplication across checker components.
- **R-TLS-004** SHOULD: Use standard library packages (bytes, context, encoding/json, net/http, time, crypto/tls, errors, fmt) for HTTP testing infrastructure without introducing third-party HTTP testing frameworks in core testing paths.
- **R-TLS-005** MUST: Document standard library package import conventions and maintain consistency in error handling patterns across all HTTP testing infrastructure.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
grep -r "import" checker/ | grep -E "(bytes|context|encoding/json|net/http|time|crypto/tls|errors|fmt)" | head -20

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
find . -name "*_test.go" -o -name "*_integration_test.go" | xargs grep -l "net/http\|crypto/tls" | head -10

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name ".golangci.yml" -o -name "lint.yaml" -o -name "staticcheck.conf" 2>/dev/null

# Verify absence of third-party HTTP testing frameworks in integration test files
grep -r "github.com.*http" go.mod go.sum 2>/dev/null | grep -v "golang.org/x/net" | head -10

# Confirm HTTP client construction uses standard library time primitives for timeouts
grep -r "http.Client" checker/ | grep -E "Timeout|time\.Duration" | head -10
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies
- No third-party HTTP testing frameworks are imported in core testing paths
- TLS configuration uses crypto/tls package exclusively for secure HTTP testing
- Shared utilities exist for header manipulation, body serialization, and error wrapping

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before accepting changes to HTTP testing infrastructure, TLS configuration, or HTTP client construction patterns.
</enforcement>