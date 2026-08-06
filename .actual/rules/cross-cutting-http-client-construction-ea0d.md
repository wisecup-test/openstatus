# Isolate External HTTP Client Construction in Boundary Layer: Http Client Construction

These rules are ALWAYS ACTIVE for all HTTP client construction code paths, including handler initialization, job execution, health check operations, and third-party API integrations.

### Rules

- **R-HTTP-001** SHOULD: HTTP client construction SHOULD apply consistent transport-level configuration including TLS settings and connection pooling parameters.

### Verify

```bash
# Locate and execute the project's integration test suite
# to verify HTTP client boundary layer behavior with test doubles
find . -name '*test*' -o -name '*spec*' | grep -E '\.(sh|py|js|go)$' | head -1 | xargs cat

# Discover the project's static analysis or linting configuration
# and verify HTTP client construction outside boundary layer is flagged
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name 'sonar-project.properties' | head -1

# Identify the project's code coverage reporting mechanism
# and verify boundary layer implementations meet coverage threshold
find . -name 'coverage.xml' -o -name '.coveragerc' -o -name 'codecov.yml' | head -1
```

**Accept when:**
- All integration tests pass with test doubles injected at the boundary layer, verifying timeout enforcement and header propagation without real network calls.
- Static analysis confirms no HTTP client construction occurs outside the designated boundary layer.
- Code coverage for boundary layer client construction and test double implementations meets or exceeds the project's defined threshold.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before marking this rule as satisfied.
</enforcement>