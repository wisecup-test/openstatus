# Isolate External HTTP Client Construction in Boundary Layer: Http Client Instances

These rules are ALWAYS ACTIVE for all files that construct HTTP client instances for communication with external endpoints, including health checks, monitoring operations, and third-party API calls.

### Rules

- **R-HTTP-001** MUST: HTTP client instances used to communicate with external endpoints MUST be constructed in a dedicated boundary layer separate from handler and job execution logic.
- **R-HTTP-002** MUST: All HTTP requests to external monitoring targets MUST use the boundary layer client construction.
- **R-HTTP-003** MUST: All HTTP requests to third-party APIs MUST use the boundary layer client construction.
- **R-HTTP-004** MUST: Ping and health check operations MUST use the boundary layer client construction.
- **R-HTTP-005** MUST: HTTP job execution paths MUST use the boundary layer client construction.
- **R-HTTP-006** SHOULD: The boundary layer interface SHOULD return both the client instance and any associated cleanup functions to ensure proper resource management and connection pooling behavior.
- **R-HTTP-007** SHOULD: Test doubles SHOULD replicate production client behavior for timeout enforcement by tracking elapsed time and returning timeout errors when thresholds are exceeded.
- **R-HTTP-008** SHOULD: The boundary layer interface SHOULD accept extensible configuration objects rather than fixed parameter lists to accommodate future transport-level requirements.

### Verify

```bash
# Locate and execute the project's integration test suite
find . -name "*test*" -o -name "*spec*" | grep -E "(integration|e2e)" | head -1 | xargs -I {} sh -c 'cd $(dirname {}) && npm test || go test ./... || python -m pytest'

# Discover and execute static analysis or linting configuration
if [ -f ".eslintrc" ] || [ -f "eslint.config.js" ]; then npm run lint; fi
if [ -f ".golangci.yml" ]; then golangci-lint run; fi
if [ -f "pylintrc" ] || [ -f "pyproject.toml" ]; then pylint . || ruff check .; fi

# Identify and verify code coverage reporting
if [ -f "coverage.json" ] || [ -d "coverage" ]; then cat coverage/coverage-summary.json | grep -E '"lines"|"statements"'; fi
if [ -f "coverage.xml" ]; then grep -E 'line-rate|branch-rate' coverage.xml; fi

# Search for inline HTTP client construction outside boundary layer
grep -r "http\.Client\|NewClient\|NewRequest" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" | grep -v "boundary\|factory\|client_factory" | wc -l
```

**Accept when:**
- All integration tests pass with test doubles injected at the boundary layer, verifying timeout enforcement and header propagation without real network calls.
- Static analysis confirms no HTTP client construction occurs outside the designated boundary layer.
- Code coverage for boundary layer client construction and test double implementations meets or exceeds the project's defined threshold.
- Grep search for inline HTTP client construction returns zero results or only approved exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All four acceptance criteria MUST be confirmed before marking this rule as satisfied.
</enforcement>