# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: System Track Last

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-APIKEY-001** SHOULD: The system SHOULD track last-used timestamps for API keys to support usage auditing and key rotation policies.

### Verify

```bash
# Discover the project's test execution mechanism and run the authentication subsystem test suite
# to verify key generation produces unique values with expected entropy.
# Command: [project-specific test runner] -- authentication subsystem tests

# Discover the project's static analysis or linting configuration and verify that no plaintext
# API key storage patterns are flagged in credential management code.
# Command: [project-specific linter] --check-credentials src/

# Discover the project's dependency scanning mechanism and verify that cryptographic and hashing
# libraries are current with no known high-severity vulnerabilities.
# Command: [project-specific dependency scanner] --fail-on high,critical
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.
- Usage tracking implementation includes timestamp precision sufficient for audit requirements without excessive database write load.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification categories (test suite, static analysis, dependency scanning) MUST pass before accepting implementation. Code review MUST include security team approval for changes to credential management logic.
</enforcement>