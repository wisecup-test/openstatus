# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Key Tokens Generated

These rules are ALWAYS ACTIVE for all code paths that generate, hash, verify, or track usage of API key credentials for authentication purposes.

### Rules

- **R-APIKEY-001** MUST: API key tokens MUST be generated using a cryptographically secure random number generator with at least 128 bits of entropy.
- **R-APIKEY-002** MUST: API keys MUST be hashed using an adaptive hashing algorithm (bcrypt with work factor ≥ 10) before storage in persistent data stores.
- **R-APIKEY-003** MUST: API key verification MUST use the hashing library's built-in constant-time comparison function; custom comparison logic is prohibited.
- **R-APIKEY-004** SHOULD: API key usage tracking SHOULD implement conditional updates with time thresholds to avoid unnecessary database writes on every authentication.
- **R-APIKEY-005** SHOULD: API key generation, hashing, and verification logic SHOULD be separated into distinct functions to enable independent unit testing and performance benchmarking.

### Verify

```bash
# Discover and run the project's test suite covering API key generation, hashing, and verification
# Confirm all tests pass and verify cryptographic properties (entropy, work factor compliance)

# Discover and execute the project's static analysis or linting configuration
# Verify no instances of plain-text credential storage or non-cryptographic random generation are detected

# Discover and run the project's dependency scanning configuration
# Confirm the adaptive hashing library is present at the locked version with no high or critical severity vulnerabilities

# Inspect the codebase for API key generation functions
# Verify they call the cryptographic random module and encode output in URL-safe format

# Inspect the codebase for API key verification functions
# Verify they use only the hashing library's built-in verification method (no custom comparison)

# Inspect usage tracking implementation
# Verify conditional update logic with time thresholds is present to minimize database writes
```

**Accept when:**
- All tests for API key generation produce tokens with sufficient entropy (≥128 bits) and all hashing tests verify work factor compliance (≥10).
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation.
- Dependency scanning confirms the adaptive hashing library is at the locked version with no high or critical severity vulnerabilities.
- Code inspection confirms API key generation uses cryptographic random generation and URL-safe encoding.
- Code inspection confirms API key verification uses only the hashing library's built-in constant-time comparison.
- Code inspection confirms usage tracking implements conditional updates with time thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-APIKEY-001 through R-APIKEY-005 are mandatory for any code path handling API key credentials. Violations detected by automated tests, static analysis, or dependency scanning MUST block merge and trigger immediate remediation.
</enforcement>