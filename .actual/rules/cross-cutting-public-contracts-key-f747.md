# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Public Contracts Key

These rules are ALWAYS ACTIVE for all authentication and credential management code that generates, stores, verifies, or tracks API keys.

### Rules

- **R-CRYPTO-001** MUST: Public contracts for key generation, hashing, and verification MUST be exposed through well-defined module exports to enable consistent usage across authentication boundaries.
- **R-CRYPTO-002** MUST: API key generation MUST use cryptographically secure random byte generation, never non-cryptographic random number generators.
- **R-CRYPTO-003** MUST: API keys MUST be stored using one-way adaptive cost hashing, never in plaintext or with reversible encryption.
- **R-CRYPTO-004** MUST: API key verification MUST use constant-time comparison functions provided by the hashing library to prevent timing attacks.
- **R-CRYPTO-005** MUST: All cryptographic library dependencies MUST be tracked in the project's dependency manifest and lock file.
- **R-CRYPTO-006** SHOULD: Cost factor configuration for adaptive hashing SHOULD be benchmarked against authentication latency requirements for the target deployment environment.
- **R-CRYPTO-007** SHOULD: Usage tracking for API keys SHOULD implement conditional update logic to prevent database write amplification during high-frequency authentication scenarios.

### Verify

```bash
# Discover the project's test suite location and execute tests covering API key generation, hashing, verification, and timing attack resistance
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(auth|key|crypto)' | head -5

# Locate the project's security scanning configuration and run dependency vulnerability checks
find . -type f \( -name 'package.json' -o -name 'Gemfile' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'pom.xml' -o -name 'Cargo.toml' \) | head -1

# Verify cryptographic library is declared in dependency manifest
grep -E '(crypto|bcrypt|argon|scrypt|sodium)' $(find . -type f \( -name 'package.json' -o -name 'Gemfile' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'pom.xml' -o -name 'Cargo.toml' \) | head -1)

# Identify the build tool and inspect lock/resolution artifact for exact resolved versions
ls -la | grep -E '(package-lock.json|yarn.lock|Gemfile.lock|poetry.lock|go.sum|Cargo.lock)'

# Measure authentication latency under load to validate cost factor configuration
grep -r 'cost\|factor\|rounds\|iterations' . --include='*.js' --include='*.py' --include='*.go' --include='*.rb' | grep -E '(hash|auth|key)' | head -10
```

**Accept when:**
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.
- Public contracts for key generation, hashing, and verification are exported through a single module interface.
- Constant-time comparison is used for all API key verification operations.
- Cost factor configuration is documented and benchmarked for the target deployment environment.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cryptographic library usage MUST be verified against the exact resolved version in the project's lock file before implementation. Security tests MUST pass and dependency scans MUST show no vulnerabilities before accepting any code that uses these rules.
</enforcement>