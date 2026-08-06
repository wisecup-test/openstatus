# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Key Tokens Hashed

These rules are ALWAYS ACTIVE for all code paths that generate, store, verify, or track usage of API key tokens for authentication purposes.

### Rules

- **R-CRYPT-001** MUST: API key tokens MUST be hashed using an adaptive key derivation function with a work factor of at least 10 before storage.
- **R-CRYPT-002** MUST: API keys MUST be generated using cryptographic random generation with sufficient entropy to prevent prediction or collision attacks.
- **R-CRYPT-003** MUST: API key verification MUST use only the hashing library's built-in constant-time comparison function; custom comparison logic is prohibited.
- **R-CRYPT-004** SHOULD: Implement usage tracking with conditional updates to avoid unnecessary database writes; consider a time threshold to update last-used timestamps only when sufficient time has elapsed.
- **R-CRYPT-005** SHOULD: Separate hashing and verification logic into distinct functions to enable independent unit testing with known test vectors and performance benchmarking.

### Verify

```bash
# Discover and run the project's test suite covering API key generation, hashing, and verification
# Confirm all tests pass and verify cryptographic properties (entropy, work factor compliance)

# Discover and execute the project's static analysis or linting configuration
# Verify security-focused rules detect no instances of plain-text credential storage or weak random generation

# Discover and run the project's dependency scanning configuration
# Confirm the adaptive hashing library is present at the locked version with no known vulnerabilities
```

**Accept when:**
- All tests for API key generation produce tokens with sufficient entropy and all hashing tests verify work factor compliance
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation
- Dependency scanning confirms the hashing library is at the locked version with no high or critical severity vulnerabilities
- Code review confirms hashing and verification logic are separated into distinct functions
- Code review confirms only the hashing library's built-in verification function is used

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for API key token handling code paths. Violations must be caught during code review and CI pipeline execution before merge.
</enforcement>