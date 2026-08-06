# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Key Usage Tracking

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem.

### Rules

- **R-APIKEY-001** SHOULD: API key usage tracking SHOULD implement shouldUpdateLastUsed logic to record authentication events without impacting verification latency.

### Verify

```bash
# Discover the project's test execution configuration and run the authentication module's test suite
# to verify API key generation produces unique tokens

# Discover the project's static analysis or linting configuration and verify that credential hashing
# operations use the required work factor

# Discover the project's dependency verification tooling and confirm the cryptographic libraries
# are pinned to exact versions in the lock artifact
```

**Accept when:**
- All API key generation tests pass, demonstrating cryptographically random token creation with sufficient entropy
- All API key verification tests pass, including constant-time comparison validation and hash mismatch handling
- Static analysis confirms no plaintext API key storage and all credential operations use the approved hashing interface
- Dependency verification confirms cryptographic libraries are locked to specific versions and match security baseline requirements

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suite execution in continuous integration MUST validate API key generation, hashing, and verification contracts. Static code analysis MUST scan for plaintext credential storage or weak random number generation. Security-focused code review MUST verify work factors and entropy sources. Dependency scanning tools MUST validate cryptographic library versions against known vulnerabilities.
</enforcement>