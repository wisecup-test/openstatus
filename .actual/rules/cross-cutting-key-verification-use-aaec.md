# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Key Verification Use

These rules are ALWAYS ACTIVE for all code paths that generate, hash, store, or verify API keys for authentication purposes.

### Rules

- **R-BCRYPT-001** MUST: API key verification MUST use constant-time comparison through the hashing library's verification function.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite covering API key generation, hashing, and verification to confirm cryptographic properties
# Discover the project's static analysis or linting configuration and execute security-focused rules to detect plain-text credential storage or weak random generation
# Discover the project's dependency scanning configuration and verify that the adaptive hashing library is present at the locked version with no known vulnerabilities
```

**Accept when:**
- All tests for API key generation produce tokens with sufficient entropy and all hashing tests verify work factor compliance
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation
- Dependency scanning confirms the hashing library is at the locked version with no high or critical severity vulnerabilities
- Code review confirms API key verification uses only the hashing library's built-in verification function with no custom comparison logic

<enforcement>
Claude Code MUST NOT skip or defer verification. All API key verification code paths MUST be audited to confirm constant-time comparison is enforced through the hashing library's verification function. Violations block merge and trigger security incident response.
</enforcement>