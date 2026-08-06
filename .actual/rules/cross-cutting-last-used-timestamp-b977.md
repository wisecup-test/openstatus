# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Last Used Timestamp

These rules are ALWAYS ACTIVE for all code implementing API key generation, hashing, verification, and usage tracking within the authentication and credential management subsystem.

### Rules

- **R-CRYPTO-001** MUST: Use cryptographically secure random byte generation for API key creation, never non-cryptographic random number generators.
- **R-CRYPTO-002** MUST: Store API keys using one-way adaptive cost hashing, never in plaintext or with reversible encryption.
- **R-CRYPTO-003** MUST: Implement constant-time comparison for API key hash verification to prevent timing attacks.
- **R-CRYPTO-004** SHOULD: Last-used timestamp tracking SHOULD implement rate-limiting logic to minimize unnecessary database writes during high-frequency authentication.
- **R-CRYPTO-005** MUST: Use only established cryptographic libraries with peer review and security track records; never implement custom cryptographic primitives.
- **R-CRYPTO-006** MUST: Export all public contracts (generateApiKey, hashApiKey, verifyApiKeyHash, shouldUpdateLastUsed) through a single module interface to maintain security boundary.
- **R-CRYPTO-007** MUST: Configure adaptive cost hashing parameters based on current hardware capabilities and authentication latency benchmarks.
- **R-CRYPTO-008** MUST: Maintain automated dependency scanning and subscribe to security advisories for all cryptographic libraries.

### Verify

```bash
# Discover the project's test suite location and execute tests covering API key generation, hashing, verification, and timing attack resistance
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(auth|crypto|key)' | head -5

# Locate the project's security scanning configuration and run dependency vulnerability checks
find . -type f \( -name 'package.json' -o -name 'Gemfile' -o -name 'pom.xml' -o -name 'go.mod' -o -name 'Cargo.toml' \) | head -1

# Identify the project's benchmarking tools and measure authentication latency under load
find . -type f -name '*bench*' -o -name '*perf*' | head -5

# Verify cryptographic library usage in authentication module
grep -r 'generateApiKey\|hashApiKey\|verifyApiKeyHash\|shouldUpdateLastUsed' . --include='*.js' --include='*.ts' --include='*.py' --include='*.go' --include='*.java' 2>/dev/null | head -10
```

**Accept when:**
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.
- All public contracts are exported through a single module interface with clear documentation.
- Constant-time comparison is verified in use for all hash verification operations.
- Cost factor configuration is documented and justified based on deployment environment benchmarks.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are non-negotiable; SHOULD rules represent strong recommendations that require documented justification if not followed. Verification commands MUST be executed before accepting any implementation claiming compliance with this ADR.
</enforcement>