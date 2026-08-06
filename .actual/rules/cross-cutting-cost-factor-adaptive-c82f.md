# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Cost Factor Adaptive

These rules are ALWAYS ACTIVE for all code implementing API key generation, hashing, verification, and usage tracking within the authentication and credential management subsystem.

### Rules

- **R-CRYPTO-001** SHOULD: The cost factor for adaptive hashing SHOULD be configured to balance security requirements with acceptable authentication latency.
- **R-CRYPTO-002** MUST: Use cryptographically secure random byte generation for API key creation, never non-cryptographic random number generators.
- **R-CRYPTO-003** MUST: Store API keys using one-way hashing only; never store in plaintext or use reversible encryption.
- **R-CRYPTO-004** MUST: Use only established cryptographic libraries with peer review and battle-testing; never implement custom cryptographic primitives.
- **R-CRYPTO-005** MUST: Use constant-time comparison functions provided by the hashing library during API key verification to prevent timing attacks.
- **R-CRYPTO-006** MUST: Export all public contracts (generateApiKey, hashApiKey, verifyApiKeyHash, shouldUpdateLastUsed) through a single module interface to maintain a clear security boundary.
- **R-CRYPTO-007** SHOULD: Implement usage tracking with conditional update logic to prevent database write amplification during high-frequency authentication scenarios.
- **R-CRYPTO-008** MUST: Before using any versioned cryptographic or hashing library, consult the exact resolved version from the repository lock artifact and verify all APIs exist in that version's official documentation.

### Verify

```bash
# Discover the project's test suite location and execute tests covering API key generation, hashing, verification, and timing attack resistance
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(auth|key|crypto)' | head -5

# Locate the project's security scanning configuration and run dependency vulnerability checks against all cryptographic libraries
grep -r 'cryptograph\|hash' package.json pom.xml build.gradle requirements.txt Gemfile Cargo.toml 2>/dev/null || echo "Dependency manifest not found"

# Identify the project's benchmarking tools and measure authentication latency under load to validate cost factor configuration
find . -type f -name '*bench*' -o -name '*perf*' | head -5

# Verify constant-time comparison is used in verification logic
grep -r 'constantTime\|timingSafe\|secure.*compare' . --include='*.js' --include='*.ts' --include='*.py' --include='*.java' --include='*.go' 2>/dev/null || echo "Constant-time comparison not found"

# Confirm cryptographic library usage is isolated in a single module
find . -type f -name '*crypto*' -o -name '*auth*' | grep -E '\.(js|ts|py|java|go)$' | head -10
```

**Accept when:**
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.
- Constant-time comparison functions are confirmed in use during API key verification.
- All cryptographic operations are routed through a single, auditable module interface.
- Cost factor configuration is documented and benchmarked for the target deployment environment.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cryptographic library usage MUST be verified against the exact resolved version from the repository lock artifact before implementation. Security tests MUST pass and dependency scans MUST be clean before code review approval.
</enforcement>