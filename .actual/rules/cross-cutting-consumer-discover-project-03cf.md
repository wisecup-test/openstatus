# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Consumer Discover Project

These rules are ALWAYS ACTIVE for all authentication credential management code, API key generation, secure hashing, and verification implementations within the project.

### Rules

- **R-CRYPTO-001** MUST: Discover the project's dependency lock file and resolve the exact locked versions of all cryptographic and hashing libraries before implementation.
- **R-CRYPTO-002** MUST: Use cryptographically secure random byte generation for API key creation, never non-cryptographic random number generators.
- **R-CRYPTO-003** MUST: Store API keys using one-way adaptive cost hashing in the database, never in plaintext or with reversible encryption.
- **R-CRYPTO-004** MUST: Implement API key verification using constant-time comparison functions provided by the hashing library to prevent timing attacks.
- **R-CRYPTO-005** MUST: Export all public contracts (generateApiKey, hashApiKey, verifyApiKeyHash, shouldUpdateLastUsed) through a single module interface to maintain a clear security boundary.
- **R-CRYPTO-006** SHOULD: Implement usage tracking with conditional update logic to prevent database write amplification during high-frequency authentication scenarios.
- **R-CRYPTO-007** SHOULD: Configure adaptive cost hashing parameters based on authentication latency benchmarks for the target deployment environment.
- **R-CRYPTO-008** SHOULD: Maintain centralized auditing of all credential lifecycle operations through the single module interface.

### Verify

```bash
# 1. Discover and verify dependency lock file
find . -name "package-lock.json" -o -name "yarn.lock" -o -name "Pipfile.lock" -o -name "go.sum" -o -name "Cargo.lock" | head -1

# 2. Extract exact versions of cryptographic libraries from lock file
# (tool-specific; consumer must adapt based on discovered build tool)

# 3. Locate and run project test suite for API key generation, hashing, verification
# Consumer must discover test runner and execute security-critical test paths

# 4. Run dependency vulnerability scanning
# Consumer must discover security scanning tool and execute against cryptographic libraries

# 5. Measure authentication latency under load
# Consumer must discover benchmarking tool and validate cost factor configuration

# 6. Verify constant-time comparison usage in verification code
grep -r "constant.time\|timing.safe\|secure.compare" . --include="*.py" --include="*.js" --include="*.go" --include="*.java" || echo "Verify manually that hashing library's constant-time comparison is used"
```

**Accept when:**
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.
- Exact locked versions of all cryptographic libraries are documented and verified against official documentation.
- All public contracts are exported through a single module interface with centralized audit logging.
- Constant-time comparison functions from the hashing library are confirmed in use for all verification operations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cryptographic library usage MUST be grounded in exact locked versions before implementation. Dependency vulnerability scanning MUST pass before any code is committed. Timing attack resistance MUST be verified through code review and testing.
</enforcement>