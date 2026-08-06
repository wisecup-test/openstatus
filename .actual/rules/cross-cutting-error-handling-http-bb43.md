# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Error Handling Http

These rules are ALWAYS ACTIVE for all HTTP checker handlers, job implementations, integration test request/response processing, external HTTP client configuration and execution, request body serialization and response body deserialization, and header manipulation and validation logic.

### Rules

- **R-HTTP-ERR-001** MUST: Error handling in HTTP test paths must use standard library error primitives for error construction and propagation.

### Verify

```bash
# Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
find . -name 'go.mod' -o -name 'go.sum' | head -1
grep -r 'import.*errors' --include='*_test.go' --include='*http*.go' | grep -v vendor

# Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
find . -name '*_test.go' -path '*/integration/*' -o -name '*integration*test*.go' | head -5

# Identify static analysis or linting configuration that enforces standard library usage patterns in test paths
find . -name '.golangci.yml' -o -name 'golangci.yml' -o -name '.golintci.yaml' | head -1
```

**Accept when:**
- All HTTP integration test files demonstrate consistent use of standard library packages (errors, fmt, encoding/json, net/http, context, time, crypto/tls, bytes) for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies
- Error handling in HTTP test paths uses standard library error primitives (errors.New, fmt.Errorf, errors.Is, errors.As) without third-party error wrapping libraries

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP test paths MUST be inspected for compliance with R-HTTP-ERR-001 before code acceptance.
</enforcement>