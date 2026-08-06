# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Http Client Implementations

These rules are ALWAYS ACTIVE for all HTTP integration test files, HTTP handler implementations, and external HTTP client configurations within the checker service.

### Rules

- **R-HTTP-001** MUST: HTTP client implementations used in integration testing must configure timeouts using standard library time primitives.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
find . -name 'go.mod' -o -name 'go.sum' | head -1
grep -r 'import.*net/http\|import.*time\|import.*encoding/json' --include='*_test.go' --include='*_integration.go' | head -20

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
go test -v ./... -run Integration 2>&1 | head -50

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.lintrc' | xargs cat 2>/dev/null

# Verify HTTP client construction patterns show explicit timeout configuration
grep -r 'http.Client{' --include='*.go' -A 5 | grep -E 'Timeout|time\.' | head -20

# Confirm absence of third-party HTTP testing frameworks in core testing paths
grep -r 'import.*httptest\|import.*testify\|import.*gock' --include='*_test.go' --include='*_integration.go' | wc -l
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages (net/http, encoding/json, time, context, bytes, errors) for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives (time.Duration, time.Second, etc.)
- Request and response body processing uses standard library JSON encoding/decoding (encoding/json package) without third-party serialization dependencies
- Dependency manifest contains no third-party HTTP testing frameworks in integration test infrastructure paths
- Static analysis confirms no violations of standard library usage patterns in test-adjacent code paths

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client implementations in integration testing MUST be inspected for explicit timeout configuration using standard library time primitives before code acceptance.
</enforcement>