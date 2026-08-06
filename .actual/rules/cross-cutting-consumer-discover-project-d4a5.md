# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Consumer Discover Project

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-CRYPTO-001** MUST: Discover the project's dependency lock artifact and resolve the exact installed versions of cryptographic and hashing libraries before implementing credential operations.
- **R-CRYPTO-002** MUST: Generate random bytes with sufficient length to produce the desired key entropy after encoding; 16 bytes yields 128 bits of entropy before base64 or hex encoding.
- **R-CRYPTO-003** MUST: Configure adaptive hashing work factors based on acceptable authentication latency SLOs; measure actual hashing time in production environment to validate configuration.
- **R-CRYPTO-004** MUST: Expose verification functions that accept both plaintext tokens and stored hashes to support testing without requiring database access.
- **R-CRYPTO-005** SHOULD: Implement usage tracking with timestamp precision sufficient for audit requirements while avoiding excessive database write load during high-frequency authentication.
- **R-CRYPTO-006** MUST: Look up the official documentation, changelog, or public API reference for the exact resolved version of each cryptographic dependency before using any API, class, or function.

### Verify

```bash
# 1. Discover the project's test execution mechanism and run the authentication subsystem test suite
# to verify key generation produces unique values with expected entropy.
echo "Running authentication subsystem tests..."
# (Exact command depends on project's build tool — discover from repository)

# 2. Discover the project's static analysis or linting configuration and verify that no plaintext
# API key storage patterns are flagged in credential management code.
echo "Running static analysis for plaintext credential storage..."
# (Exact command depends on project's linting tool — discover from repository)

# 3. Discover the project's dependency scanning mechanism and verify that cryptographic and
# hashing libraries are current with no known high-severity vulnerabilities.
echo "Scanning cryptographic dependencies for vulnerabilities..."
# (Exact command depends on project's dependency scanner — discover from repository)
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification categories (test execution, static analysis, dependency scanning) MUST complete successfully before accepting implementation. Violations block merge in CI pipeline.
</enforcement>