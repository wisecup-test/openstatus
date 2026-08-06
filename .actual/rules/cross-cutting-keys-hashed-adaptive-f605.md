# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Keys Hashed Adaptive

These rules are ALWAYS ACTIVE for all code implementing API key generation, hashing, verification, and usage tracking within the authentication and credential management subsystem.

### Rules

- **R-CRYPT-001** MUST: API keys MUST be hashed using adaptive cost factor algorithms before storage in credential repositories.

### Verify

```bash
# Discover the project's test suite location and execute tests covering API key generation, hashing, verification, and timing attack resistance.
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(key|auth|cred)' | head -20

# Locate the project's security scanning configuration and run dependency vulnerability checks against all cryptographic libraries.
find . -type f \( -name 'package.json' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Gemfile' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Cargo.toml' \)

# Identify the project's benchmarking tools and measure authentication latency under load to validate cost factor configuration.
find . -type f -name '*bench*' -o -name '*perf*' | head -10
```

**Accept when:**
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.

<enforcement>
Claude Code MUST NOT skip or defer verification. All security-critical code paths must be tested and verified before acceptance. Dependency scanning is mandatory on every build.
</enforcement>