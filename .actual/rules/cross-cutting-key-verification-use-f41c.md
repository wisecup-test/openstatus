# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Key Verification Use

These rules are ALWAYS ACTIVE for all public API endpoints requiring authentication, service-to-service authentication mechanisms, client credential generation and management workflows, and API key lifecycle operations including creation, verification, and revocation.

### Rules

- **R-KEYVER-001** MUST: API key verification must use constant-time comparison operations provided by the hashing library to prevent timing attacks.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite covering credential generation, hashing, and verification functions
# Locate the project's static analysis or linting configuration and execute security-focused rules checking for plaintext credential storage
# Identify the project's dependency scanning tooling and verify no known vulnerabilities exist in cryptographic or hashing dependencies
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked by CI checks. Security scanning alerts trigger immediate review and remediation workflows for vulnerable dependency versions. Code review guidelines require explicit verification of cryptographic library usage and work factor configuration.
</enforcement>