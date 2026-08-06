# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Verification Use

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-CRYPTO-001** MUST: API key verification MUST use constant-time comparison operations provided by the hashing library.

### Verify

```bash
# Discover the project's test execution mechanism and run the authentication subsystem test suite
# to verify key generation produces unique values with expected entropy.
echo "Running authentication subsystem tests..."
# (Exact command depends on project's build tool; inspect lock file and manifest)

# Discover the project's static analysis or linting configuration and verify that no plaintext
# API key storage patterns are flagged in credential management code.
echo "Running static analysis for plaintext credential storage..."
# (Exact command depends on project's linting tool; inspect configuration)

# Discover the project's dependency scanning mechanism and verify that cryptographic and hashing
# libraries are current with no known high-severity vulnerabilities.
echo "Scanning cryptographic dependencies for vulnerabilities..."
# (Exact command depends on project's dependency scanner; inspect CI configuration)
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before approving changes to credential management logic. Code review process requires security team approval for changes to this subsystem.
</enforcement>