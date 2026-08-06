# Standardize Structured Logging with zerolog for Observability: Http Handler Functions

These rules are ALWAYS ACTIVE for all HTTP handler functions, DNS and TCP checker handlers, job execution functions, client wrappers for external services, retry and backoff logic, and server initialization code.

### Rules

- **R-ZLOG-001** SHOULD: HTTP handler functions SHOULD attach request identifiers and event metadata to the context before passing to downstream operations.

### Verify

```bash
# Discover and execute the project's static analysis tooling to verify all error logging calls use structured logging methods
find . -name '*.go' -type f | xargs grep -l 'logger\.Error\|log\.Ctx' | head -20

# Locate the project's test suite and run integration tests that validate context propagation through handler chains
go test -v ./... -run TestHandler -timeout 30s

# Identify the project's linting configuration and verify rules enforce context-aware logging patterns
if [ -f .golangci.yml ] || [ -f golangci.yml ]; then golangci-lint run ./...; fi
```

**Accept when:**
- All error logging statements in checker handlers and job executors use structured logging with error object attachment
- Context propagation is verified through the request lifecycle from handler entry to error logging
- Static analysis confirms no string concatenation or formatting is used for error message construction where structured fields are available

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations are caught by automated static analysis in the CI pipeline, code review checklist enforcement, and integration tests validating log output format and field presence.
</enforcement>