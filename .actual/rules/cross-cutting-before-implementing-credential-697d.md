# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Before Implementing Credential

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem.

### Rules

- **R-CRED-001** MUST: Before implementing credential hashing, discover the project's dependency lock artifact, resolve the exact installed version of the bcrypt library, and verify API compatibility against that version's official documentation.
- **R-CRED-002** MUST: Generate API keys by encoding random bytes from crypto.randomBytes to URL-safe base64 or hexadecimal format to ensure compatibility with HTTP headers and query parameters.
- **R-CRED-003** MUST: Return the plaintext API key to the user exactly once during generation, as the stored hash cannot be reversed for recovery.
- **R-CRED-004** MUST: Use bcrypt.hash with work factor 10 for all API key storage operations.
- **R-CRED-005** MUST: Implement constant-time comparison for API key verification to prevent timing side-channel attacks.
- **R-CRED-006** SHOULD: Implement shouldUpdateLastUsed with asynchronous or background processing to avoid blocking the authentication response path.
- **R-CRED-007** SHOULD: Implement rate limiting on API key verification attempts to mitigate online brute-force attacks independent of hash strength.
- **R-CRED-008** SHOULD: Maintain an abstraction layer for credential operations to enable future migration away from bcryptjs if needed.

### Verify

```bash
# 1. Discover the project's dependency lock artifact and resolve bcrypt version
# (Consumer must identify lock file based on build tool: package-lock.json, yarn.lock, pnpm-lock.yaml, etc.)
grep -A 2 'bcryptjs' <lock-artifact> | head -5

# 2. Verify bcrypt library version matches documentation
# (Consumer must fetch official bcryptjs documentation for resolved version)
echo "Confirm resolved bcryptjs version against official API documentation"

# 3. Run authentication module test suite
# (Consumer must discover test execution configuration)
echo "Execute authentication module tests to verify API key generation produces unique tokens"

# 4. Verify static analysis configuration
# (Consumer must discover linting/analysis tool configuration)
echo "Run static analysis to confirm credential hashing uses required work factor"

# 5. Verify dependency pinning
# (Consumer must run dependency verification tooling)
echo "Confirm cryptographic libraries are pinned to exact versions in lock artifact"
```

**Accept when:**
- All API key generation tests pass, demonstrating cryptographically random token creation with sufficient entropy
- All API key verification tests pass, including constant-time comparison validation and hash mismatch handling
- Static analysis confirms no plaintext API key storage and all credential operations use the approved hashing interface
- Dependency verification confirms cryptographic libraries are locked to specific versions and match security baseline requirements
- Official bcryptjs documentation for the resolved version confirms all called APIs exist and work factor parameter is supported

<enforcement>
Claude Code MUST NOT skip or defer verification. The LOCK-VERSION GROUNDING policy is mandatory: before writing code that uses bcryptjs, execute discovery steps 1–5 in order to confirm exact version compatibility. All R-CRED-### rules prefixed MUST are non-negotiable. Violations block CI pipeline and trigger security team notification.
</enforcement>