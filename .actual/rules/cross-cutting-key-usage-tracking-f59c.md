# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Key Usage Tracking

These rules are ALWAYS ACTIVE for all code implementing API key generation, hashing, verification, and usage tracking in authentication flows.

### Rules

- **R-BCRYPT-001** MUST: Generate API keys using cryptographic random generation from the system's cryptographic module, not pseudo-random or weak random sources.
- **R-BCRYPT-002** MUST: Hash API keys using adaptive hashing (bcrypt with work factor of 10 or higher) before storage in persistent data stores.
- **R-BCRYPT-003** MUST: Never store plain-text API keys in the database; only store the hashed representation.
- **R-BCRYPT-004** MUST: Use the hashing library's built-in verification function for constant-time comparison during authentication; do not implement custom comparison logic.
- **R-BCRYPT-005** SHOULD: API key usage tracking SHOULD implement rate limiting to prevent excessive verification attempts within a time window.
- **R-BCRYPT-006** SHOULD: Implement conditional updates to usage tracking (last-used timestamps) with a time threshold to avoid unnecessary database writes on every verification.
- **R-BCRYPT-007** MUST: Separate API key generation, hashing, and verification into distinct functions to enable independent testing and modular security validation.
- **R-BCRYPT-008** MUST: Return both the plain token to the client and the hashed version for storage during key generation; never return the hashed version to the client.
- **R-BCRYPT-009** MUST: Establish an annual review cycle to assess work factor adequacy as computational power increases; implement versioned hashing to support gradual migration to higher work factors.
- **R-BCRYPT-010** MUST: Enable automated dependency scanning in CI pipeline for the adaptive hashing library; subscribe to security advisories and maintain an update process for security patches.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite
# covering API key generation, hashing, and verification
echo "Running API key cryptographic tests..."
# (Exact command depends on project's test runner; discover from build config)

# Discover the project's static analysis or linting configuration
# and execute security-focused rules to detect plain-text credential storage
echo "Running static analysis for credential storage violations..."
# (Exact command depends on project's linter/analyzer; discover from build config)

# Discover the project's dependency scanning configuration
# and verify the adaptive hashing library is present at locked version
echo "Scanning dependencies for hashing library vulnerabilities..."
# (Exact command depends on project's dependency scanner; discover from lock file)
```

**Accept when:**
- All tests for API key generation produce tokens with sufficient entropy (verified by entropy analysis or test assertions)
- All hashing tests verify work factor compliance (bcrypt work factor ≥ 10)
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation
- Dependency scanning confirms the hashing library is at the locked version with no high or critical severity vulnerabilities
- Code review confirms separation of generation, hashing, and verification into distinct functions
- Usage tracking implementation includes rate limiting and conditional update logic

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting implementation. Rules marked SHOULD represent strong recommendations that should be followed unless documented exceptions exist with security team approval.
</enforcement>