# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Implementations Increase Bcrypt

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem, including all files in the authentication module that expose public contracts for credential management.

### Rules

- **R-BCRYPT-001** MAY: Implementations MAY increase the bcrypt work factor beyond 10 based on security requirements and performance constraints.
- **R-BCRYPT-002** MUST: Generate API keys by encoding random bytes to URL-safe base64 or hexadecimal format to ensure compatibility with HTTP headers and query parameters.
- **R-BCRYPT-003** MUST: Return the plaintext API key to the user exactly once during generation, as the stored hash cannot be reversed for recovery.
- **R-BCRYPT-004** SHOULD: Implement shouldUpdateLastUsed with asynchronous or background processing to avoid blocking the authentication response path.
- **R-BCRYPT-005** SHOULD: Consider implementing rate limiting on API key verification attempts to mitigate online brute-force attacks independent of hash strength.
- **R-BCRYPT-006** MUST: Use crypto.randomBytes for entropy generation in all API key creation operations.
- **R-BCRYPT-007** MUST: Use bcryptjs for hashing all API keys before storage in credential databases.
- **R-BCRYPT-008** MUST NOT: Store API keys in plaintext or reversibly encrypted form.

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
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication subsystem implementations. Violations block CI pipeline merges and trigger security team notification.
</enforcement>