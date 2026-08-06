# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Generation Use

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-KEYGEN-001** MUST: API key generation MUST use cryptographic random number generation with at least 128 bits of entropy.

### Verify

```bash
# Discover the project's test execution mechanism and run the authentication subsystem test suite
# to verify key generation produces unique values with expected entropy.
echo "Running authentication subsystem tests for key generation uniqueness and entropy..."

# Discover the project's static analysis or linting configuration and verify that no plaintext
# API key storage patterns are flagged in credential management code.
echo "Running static analysis to verify no plaintext credential storage patterns..."

# Discover the project's dependency scanning mechanism and verify that cryptographic and hashing
# libraries are current with no known high-severity vulnerabilities.
echo "Running dependency vulnerability scan on cryptographic dependencies..."
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before proceeding with implementation.
</enforcement>