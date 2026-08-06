# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Implementation Expose Separate

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-APIK-001** MAY: The implementation MAY expose separate public contracts for key generation, hashing, verification, and usage tracking to support modular testing and reuse.

### Verify

```bash
# Discover the project's test execution mechanism and run the authentication subsystem test suite
# to verify key generation produces unique values with expected entropy.
# Verify commands:
# 1. Find and execute the authentication subsystem test suite
# 2. Confirm key generation uniqueness and entropy validation tests pass
# 3. Run static analysis to verify no plaintext API key storage patterns are flagged
# 4. Execute dependency scanning to verify cryptographic libraries are current with no high-severity vulnerabilities
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.
- Key generation produces values with sufficient entropy (16 bytes minimum for 128 bits before encoding).
- Adaptive hashing work factors are configured based on acceptable authentication latency SLOs.
- Verification functions accept both plaintext tokens and stored hashes for testing support.

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication subsystem tests, static analysis checks, and dependency scans MUST pass before accepting implementation. Code review MUST include security team approval for credential management logic changes.
</enforcement>