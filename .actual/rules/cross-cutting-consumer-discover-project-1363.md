# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Consumer Discover Project

These rules are ALWAYS ACTIVE for all code implementing API key generation, hashing, verification, and usage tracking in authentication systems.

### Rules

- **R-BCRYPT-001** MUST: The consumer MUST discover the project's dependency lock file and resolve the exact locked version of the adaptive hashing library before implementation.
- **R-BCRYPT-002** MUST: API keys MUST be generated using cryptographic random generation with sufficient entropy, encoded in URL-safe format, and returned as both plain token (to client) and hashed version (for storage).
- **R-BCRYPT-003** MUST: API keys MUST be stored using adaptive hashing with a work factor of at least 10 to resist offline brute-force attacks.
- **R-BCRYPT-004** MUST: API key verification MUST use only the hashing library's built-in verification function to enforce constant-time comparison and prevent timing attacks.
- **R-BCRYPT-005** MUST: Plain-text credential storage is prohibited; all API keys in persistent data stores MUST be hashed.
- **R-BCRYPT-006** MUST: Hashing and verification logic MUST be separated into distinct functions to enable independent unit testing and security validation.
- **R-BCRYPT-007** SHOULD: Usage tracking SHOULD implement conditional updates with time thresholds to avoid unnecessary database writes on every authentication.
- **R-BCRYPT-008** MAY: Development and testing environments MAY use reduced work factors to improve test suite performance (EXC-001).

### Verify

```bash
# 1. Discover and validate the dependency lock file
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' -o -name 'poetry.lock' \) | head -1

# 2. Extract the exact locked version of the adaptive hashing library
# (Tool and command vary by build system; consumer must discover from manifest)

# 3. Run the test suite covering API key generation, hashing, and verification
# (Consumer must discover test execution configuration)

# 4. Verify cryptographic properties: all generated tokens have sufficient entropy
# (Consumer must identify test framework and run relevant test suite)

# 5. Execute static analysis or linting with security-focused rules
# (Consumer must discover static analysis configuration)

# 6. Verify no plain-text credential storage or weak random generation detected

# 7. Run dependency scanning to confirm hashing library at locked version
# (Consumer must discover dependency scanning tool configuration)

# 8. Confirm no high or critical severity vulnerabilities in hashing library
```

**Accept when:**
- The exact locked version of the adaptive hashing library is identified and documented
- All tests for API key generation produce tokens with sufficient entropy
- All hashing tests verify work factor compliance (minimum 10)
- Static analysis reports no instances of plain-text credential storage
- Static analysis reports no use of non-cryptographic random generation
- Dependency scanning confirms the hashing library is at the locked version
- Dependency scanning confirms no high or critical severity vulnerabilities
- Code review confirms hashing and verification logic are in separate functions
- Code review confirms only the hashing library's built-in verification function is used

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before code is written. The LOCK-VERSION GROUNDING policy is mandatory: before using any versioned library, steps 1–5 must be executed in order, with version-specific documentation fetched from the public internet, not training data recall.
</enforcement>