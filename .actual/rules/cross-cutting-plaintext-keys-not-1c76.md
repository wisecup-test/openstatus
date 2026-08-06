# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Plaintext Keys Not

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the authentication subsystem.

### Rules

- **R-APIKEY-001** MUST NOT: Plaintext API keys MUST NOT be stored in any persistent storage system.
- **R-APIKEY-002** MUST: Generate random bytes with sufficient length to produce the desired key entropy after encoding; 16 bytes yields 128 bits of entropy before base64 or hex encoding.
- **R-APIKEY-003** MUST: Configure adaptive hashing work factors based on acceptable authentication latency SLOs; measure actual hashing time in production environment to validate configuration.
- **R-APIKEY-004** MUST: Expose verification functions that accept both plaintext tokens and stored hashes to support testing without requiring database access.
- **R-APIKEY-005** SHOULD: Implement usage tracking with timestamp precision sufficient for audit requirements while avoiding excessive database write load during high-frequency authentication.

### Verify

```bash
# Discover and run the authentication subsystem test suite
# Verify key generation produces unique values with expected entropy
echo "Running authentication subsystem tests..."
# (Project-specific test command to be discovered from build configuration)

# Discover and run static analysis or linting configuration
# Verify no plaintext API key storage patterns are flagged
echo "Running static analysis for plaintext credential storage..."
# (Project-specific linting command to be discovered from configuration)

# Discover and run dependency scanning mechanism
# Verify cryptographic and hashing libraries are current with no known high-severity vulnerabilities
echo "Scanning cryptographic dependencies for vulnerabilities..."
# (Project-specific dependency scan command to be discovered from CI configuration)
```

**Accept when:**
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification categories (test suite, static analysis, dependency scanning) MUST pass before accepting changes to credential management logic. Code review checklist MUST require verification of cryptographic library usage and work factor configuration. CI pipeline failures MUST block merge for test failures, static analysis violations, or dependency vulnerabilities.
</enforcement>