# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Authentication Module Expose

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem.

### Rules

- **R-AUTH-001** MUST: The authentication module MUST expose public contracts for generateApiKey, hashApiKey, verifyApiKeyHash, and shouldUpdateLastUsed operations.
- **R-AUTH-002** MUST: Generate API keys by encoding random bytes from crypto.randomBytes to URL-safe base64 or hexadecimal format to ensure compatibility with HTTP headers and query parameters.
- **R-AUTH-003** MUST: Return the plaintext API key to the user exactly once during generation, as the stored hash cannot be reversed for recovery.
- **R-AUTH-004** MUST: Use bcryptjs for adaptive hashing with configurable work factors (minimum work factor 10) for all API key storage operations.
- **R-AUTH-005** MUST: Implement constant-time comparison during API key verification to prevent timing side-channel attacks.
- **R-AUTH-006** SHOULD: Implement shouldUpdateLastUsed with asynchronous or background processing to avoid blocking the authentication response path.
- **R-AUTH-007** SHOULD: Implement rate limiting on API key verification attempts to mitigate online brute-force attacks independent of hash strength.
- **R-AUTH-008** SHOULD: Establish periodic security review cycle to evaluate work factor adequacy and implement credential rehashing on authentication to transparently upgrade existing hashes.
- **R-AUTH-009** SHOULD: Implement authentication caching with short TTLs and monitor verification latency metrics to prevent CPU bottlenecks during bcrypt verification.
- **R-AUTH-010** SHOULD: Pin exact versions of cryptographic libraries in lock artifacts and maintain abstraction layer for credential operations to enable future migration.

### Verify

```bash
# 1. Discover the project's test execution configuration and run the authentication module's test suite
# Verify API key generation produces unique tokens
echo "Running authentication module test suite..."
# (Exact command depends on project's build tool — inspect package.json, Makefile, or build config)

# 2. Discover the project's static analysis or linting configuration
# Verify that credential hashing operations use the required work factor
echo "Running static analysis on credential hashing operations..."
# (Exact command depends on project's linter — inspect .eslintrc, tsconfig, or similar)

# 3. Discover the project's dependency verification tooling
# Confirm the cryptographic libraries are pinned to exact versions in the lock artifact
echo "Verifying cryptographic library versions in lock artifact..."
# (Inspect package-lock.json, yarn.lock, or equivalent lock file for exact bcryptjs version)
```

**Accept when:**
- All API key generation tests pass, demonstrating cryptographically random token creation with sufficient entropy
- All API key verification tests pass, including constant-time comparison validation and hash mismatch handling
- Static analysis confirms no plaintext API key storage and all credential operations use the approved hashing interface
- Dependency verification confirms cryptographic libraries are locked to specific versions and match security baseline requirements
- Work factor is verified to be at least 10 in all bcryptjs.hash() calls
- API key generation uses crypto.randomBytes as the entropy source

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are non-negotiable. Static analysis and test execution MUST complete successfully before accepting any authentication module changes. Dependency versions MUST be verified against the lock artifact, not training data or package manifests.
</enforcement>