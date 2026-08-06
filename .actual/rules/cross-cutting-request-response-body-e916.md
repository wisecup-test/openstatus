# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Request Response Body

These rules are ALWAYS ACTIVE for all HTTP checker handlers, job implementations, integration test request/response processing, external HTTP client configuration, and request/response body serialization code paths.

### Rules

- **R-STDLIB-HTTP-001** MUST: Request and response body handling in test scenarios must use standard library serialization primitives for JSON encoding and decoding.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
find . -name 'go.mod' -o -name 'go.sum' | head -1
grep -r 'encoding/json\|net/http\|bytes\|context' --include='*_test.go' --include='*handler*.go' --include='*job*.go' | grep -E '^[^:]+:(import|\s+"(encoding/json|net/http|bytes|context))' | wc -l

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
go test -v ./... -run Integration 2>&1 | grep -E '(PASS|FAIL|encoding/json|net/http)'

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name '.golangci.yml' -o -name 'golangci.toml' -o -name '.golintci.yaml' | xargs cat 2>/dev/null
grep -r 'json\|http' --include='*.go' | grep -v 'third.party\|external' | head -20
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages for encoding, networking, and context management (encoding/json, net/http, bytes, context, time, crypto/tls, errors, fmt)
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies
- Dependency manifest contains no third-party HTTP testing frameworks in core testing paths
- Code review verification confirms HTTP client construction and configuration patterns align with standard library patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis of import statements, code review verification of HTTP client patterns, and dependency manifest inspection are mandatory before accepting code that modifies HTTP testing infrastructure.
</enforcement>