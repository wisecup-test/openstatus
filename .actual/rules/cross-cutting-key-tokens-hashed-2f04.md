# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Key Tokens Hashed

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem.

### Rules

- **R-AUTH-001** MUST: API key tokens MUST be hashed using bcrypt with a minimum work factor of 10 before storage.
- **R-AUTH-002** MUST: API key tokens MUST be generated using crypto.randomBytes to ensure cryptographically secure entropy.
- **R-AUTH-003** MUST: Plaintext API keys MUST be returned to the user exactly once during generation, as stored hashes cannot be reversed for recovery.
- **R-AUTH-004** MUST: API key verification operations MUST use constant-time comparison to prevent timing side-channel attacks.
- **R-AUTH-005** SHOULD: API key generation SHOULD encode random bytes to URL-safe base64 or hexadecimal format for HTTP header and query parameter compatibility.
- **R-AUTH-006** SHOULD: shouldUpdateLastUsed tracking SHOULD use asynchronous or background processing to avoid blocking the authentication response path.
- **R-AUTH-007** SHOULD: Rate limiting on API key verification attempts SHOULD be implemented to mitigate online brute-force attacks independent of hash strength.

### Verify

```bash
# Discover the project's test execution configuration and run the authentication module's test suite
# to verify API key generation produces unique tokens
echo "Running authentication module test suite..."
# (Exact command depends on project's build tool — inspect package.json or build manifest)

# Discover the project's static analysis or linting configuration and verify that credential
# hashing operations use the required work factor
echo "Running static analysis on credential hashing operations..."
# (Exact command depends on project's linting tool — inspect configuration files)

# Discover the project's dependency verification tooling and confirm the cryptographic
# libraries are pinned to exact versions in the lock artifact
echo "Verifying cryptographic library versions in lock artifact..."
# (Exact command depends on project's package manager — inspect lock file)
```

**Accept when:**
- All API key generation tests pass, demonstrating cryptographically random token creation with sufficient entropy
- All API key verification tests pass, including constant-time comparison validation and hash mismatch handling
- Static analysis confirms no plaintext API key storage and all credential operations use the approved hashing interface
- Dependency verification confirms cryptographic libraries are locked to specific versions and match security baseline requirements
- bcrypt work factor is verified to be at least 10 in all hashing operations
- API key tokens are confirmed to be generated using crypto.randomBytes

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication subsystem code. Violations block CI pipeline merges and trigger security team notification.
</enforcement>