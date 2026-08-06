# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Key Tokens Generated

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem.

### Rules

- **R-APIKEY-001** MUST: API key tokens MUST be generated using cryptographically secure random byte generation with minimum 16-byte entropy.
- **R-APIKEY-002** MUST: API keys MUST be stored as irreversible hashes using bcryptjs with adaptive work factors, never in plaintext or reversibly encrypted form.
- **R-APIKEY-003** MUST: Return the plaintext API key to the user exactly once during generation, as the stored hash cannot be reversed for recovery.
- **R-APIKEY-004** MUST: Generate API keys by encoding random bytes to URL-safe base64 or hexadecimal format to ensure compatibility with HTTP headers and query parameters.
- **R-APIKEY-005** SHOULD: Implement shouldUpdateLastUsed with asynchronous or background processing to avoid blocking the authentication response path.
- **R-APIKEY-006** SHOULD: Implement rate limiting on API key verification attempts to mitigate online brute-force attacks independent of hash strength.
- **R-APIKEY-007** SHOULD: Implement authentication caching with short TTLs and monitor verification latency metrics to prevent CPU bottlenecks during bcrypt verification.
- **R-APIKEY-008** SHOULD: Establish periodic security review cycle to evaluate work factor adequacy and implement credential rehashing on authentication to transparently upgrade existing hashes.

### Verify

```bash
# Discover the project's test execution configuration and run the authentication module's test suite
# to verify API key generation produces unique tokens
echo "Running authentication module test suite..."
# (Exact command depends on project's build tool — inspect package.json or equivalent)

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

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be enforced before code is committed. Static analysis and test execution are required gates in the continuous integration pipeline.
</enforcement>