# Standardize Structured Error Logging with Context Propagation: Before Any Structured

These rules are ALWAYS ACTIVE for all handler functions that process HTTP, DNS, TCP, or ping monitoring requests; client interaction code that performs external HTTP requests or network operations; retry logic implementations using backoff strategies; public API contract implementations that expose error states; and integration points with third-party APIs where errors must be captured.

### Rules

- **R-LOGGING-001** MUST: Before using any structured logging library API, discover the project's dependency lock file, resolve the exact installed version, and verify all method signatures against that version's official documentation.
- **R-LOGGING-002** MUST: Establish a context propagation pattern where handler entry points extract or create request correlation identifiers and attach them to the context object before passing to downstream functions.
- **R-LOGGING-003** MUST: Define and use standard structured field names for common error contexts such as operation type, retry attempt, external endpoint, and monitor identifier to ensure consistent queryability.
- **R-LOGGING-004** SHOULD: For retry loops with backoff strategies, log errors at the final failure point rather than every attempt, or implement sampling to reduce log volume while preserving diagnostic value.
- **R-LOGGING-005** MUST: All error logging call sites in handler functions use context-aware structured logging methods with error field extraction.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock file
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' | head -5

# 2. Identify the exact resolved version of the structured logging library
grep -E '(zerolog|log)' go.sum 2>/dev/null | head -3

# 3. Verify context-aware error logging patterns across handler implementations
grep -r 'log\.Ctx(ctx)\.Error\|logger\.Error' --include='*.go' | wc -l

# 4. Check for unstructured error logging (string concatenation patterns)
grep -r 'log\.Print\|fmt\.Errorf.*log\|errors\.New.*log' --include='*.go' | wc -l

# 5. Locate and run integration tests for protocol handlers
find . -path '*/test*' -name '*handler*test.go' -o -name '*integration*test.go' | head -5

# 6. Verify correlation identifiers are propagated in error logs
grep -r 'correlation\|request.*id\|trace.*id' --include='*.go' | grep -i log | wc -l
```

**Accept when:**
- All error logging call sites in handler functions use context-aware structured logging methods with error field extraction
- Static analysis confirms no error logging uses string concatenation or unstructured output methods
- Integration tests verify that error logs from retry operations include correlation identifiers and can be queried by structured fields
- The exact version of the structured logging library is documented and all API calls match that version's official documentation
- Standard structured field names are consistently used across all handler implementations
- Retry loop error logging implements sampling or rate-limiting to prevent log volume overflow

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory before any structured logging library API is used. Verification commands MUST be executed to confirm compliance with R-LOGGING-001 through R-LOGGING-005 before code is committed.
</enforcement>