# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Plain Text Key

These rules are ALWAYS ACTIVE for all code paths that generate, hash, store, verify, or track usage of API keys for authentication purposes.

### Rules

- **R-APIKEY-001** MUST_NOT: Plain-text API key tokens MUST_NOT be persisted in any storage system after initial generation.
- **R-APIKEY-002** MUST: API keys MUST be generated using cryptographic random generation with sufficient entropy to prevent prediction or collision attacks.
- **R-APIKEY-003** MUST: API keys MUST be hashed using an adaptive hashing algorithm (bcrypt with work factor of 10 or higher) before storage in persistent data stores.
- **R-APIKEY-004** MUST: API key verification MUST use only the hashing library's built-in constant-time comparison function; custom comparison logic is prohibited.
- **R-APIKEY-005** SHOULD: Usage tracking SHOULD implement conditional updates with time thresholds to avoid unnecessary database writes on every authentication.
- **R-APIKEY-006** SHOULD: Hashing and verification logic SHOULD be separated into distinct functions to enable independent unit testing and performance benchmarking.

### Verify

```bash
# Discover and run the project's test suite covering API key generation, hashing, and verification
# Confirm all tests pass and verify cryptographic properties (entropy, work factor compliance)
echo "Running API key cryptographic tests..."
# (Exact command depends on project's test runner; discovered from repo configuration)

# Discover and execute static analysis or linting configuration
# Verify no instances of plain-text credential storage or weak random generation are detected
echo "Running security-focused static analysis..."
# (Exact command depends on project's linter/analyzer; discovered from repo configuration)

# Discover and run dependency scanning configuration
# Verify the adaptive hashing library is present at the locked version with no known vulnerabilities
echo "Scanning dependencies for vulnerabilities..."
# (Exact command depends on project's dependency scanner; discovered from repo configuration)
```

**Accept when:**
- All tests for API key generation produce tokens with sufficient entropy and all hashing tests verify work factor compliance (minimum 10).
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation.
- Dependency scanning confirms the adaptive hashing library is at the locked version with no high or critical severity vulnerabilities.
- Code review confirms hashing and verification use only the library's built-in functions with no custom comparison logic.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for API key handling code. Violations detected by automated tests, static analysis, or security scanning MUST block merge. Exception requests require security team review and approval with documented compensating controls.
</enforcement>