# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Header Manipulation Operations

These rules are ALWAYS ACTIVE for all HTTP checker handlers, job implementations, integration test request/response processing, external HTTP client configuration, and header manipulation logic within the checker service.

### Rules

- **R-HTTP-HDR-001** SHOULD: Header manipulation operations in test request preparation should use standard library header access methods.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
find . -name 'go.mod' -o -name 'go.sum' | head -1
grep -r 'net/http' --include='*_test.go' --include='*.go' | grep -E '(Header|Set|Get|Del)' | head -20

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
go test -v ./... -run Integration 2>&1 | head -50

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name '.golangci.yml' -o -name 'golangci.yml' -o -name '.golintci.yml' | xargs cat 2>/dev/null
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages (net/http, encoding/json, context, time, crypto/tls, errors, fmt) for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies
- Header manipulation in test request preparation uses net/http.Header methods (Set, Get, Del, Add) rather than third-party utilities
- No third-party HTTP testing frameworks are present in core integration test paths

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP header manipulation in test request preparation MUST be inspected for conformance to standard library patterns before code acceptance.
</enforcement>