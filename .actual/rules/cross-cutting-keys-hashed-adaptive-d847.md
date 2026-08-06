# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Keys Hashed Adaptive

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-KEYS-001** MUST: API keys MUST be hashed using an adaptive hashing algorithm with a minimum work factor of 10 before storage.

### Verify

```bash
# Discover the project's test execution mechanism and run the authentication subsystem test suite
# to verify key generation produces unique values with expected entropy.
# Example: run tests matching pattern *auth* or *key*

# Discover the project's static analysis or linting configuration and verify that no plaintext
# API key storage patterns are flagged in credential management code.

# Discover the project's dependency scanning mechanism and verify that cryptographic and hashing
# libraries are current with no known high-severity vulnerabilities.
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication subsystem tests, static analysis checks, and dependency scans MUST pass before accepting changes to credential management logic. Code review by security team is mandatory for all modifications to API key generation, hashing, or verification operations.
</enforcement>