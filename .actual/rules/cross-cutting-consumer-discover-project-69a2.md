# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Consumer Discover Project

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files in the authentication and credential management subsystems.

### Rules

- **R-CRYPTO-001** MUST: Discover the project's dependency lock file and resolve the exact installed versions of cryptographic and hashing libraries before implementation.
- **R-CRYPTO-002** MUST: Use cryptographic random generation for all API key generation operations exposed through public contracts.
- **R-CRYPTO-003** MUST: Hash all API keys before storage using bcrypt with adaptive cost factors.
- **R-CRYPTO-004** MUST: Implement API key verification operations using constant-time comparison by delegating to the hashing library's built-in compare function.
- **R-CRYPTO-005** MUST: Never log or transmit plaintext tokens after initial generation response.
- **R-CRYPTO-006** MUST: Implement generation, hashing, and verification functions as separate, testable units with clear contracts.
- **R-CRYPTO-007** MUST: Verify the exact API signature and encoding options of the cryptographic library's secure random byte generation function in the resolved version's documentation before implementation.
- **R-CRYPTO-008** MUST: Verify the hash and compare function signatures and cost factor parameter format and valid range for the resolved bcrypt library version before implementation.
- **R-CRYPTO-009** SHOULD: Establish annual review cycle for bcrypt cost factor adequacy to maintain minimum 100ms verification time on current hardware.
- **R-CRYPTO-010** SHOULD: Implement hash upgrade mechanism that transparently re-hashes credentials with higher cost on next successful authentication.

### Verify

```bash
# 1. Discover and verify dependency lock file
find . -name "*.lock" -o -name "Gemfile.lock" -o -name "package-lock.json" -o -name "poetry.lock" -o -name "Cargo.lock" | head -5

# 2. Identify build tool and inspect lock artifact for exact resolved versions
grep -E "(bcrypt|crypto|random)" <lock-file> | head -20

# 3. Discover test suite location and run API key tests
find . -path "*/test*" -name "*api*key*" -o -path "*/spec*" -name "*api*key*" | head -10
# Execute test runner for API key generation, hashing, and verification

# 4. Discover static analysis configuration
find . -name ".eslintrc*" -o -name "pylintrc" -o -name ".rubocop.yml" -o -name "clippy.toml" | head -5
# Execute security-focused linting rules

# 5. Discover dependency scanning tool
find . -name "*dependabot*" -o -name "*snyk*" -o -name "*safety*" -o -name "*audit*" | head -5
# Execute vulnerability checks against cryptographic and hashing libraries

# 6. Verify no plaintext API key storage
grep -r "api.key\|apiKey\|API_KEY" . --include="*.py" --include="*.js" --include="*.rb" --include="*.rs" | grep -v "hash\|bcrypt\|encrypt" | head -20

# 7. Verify cryptographic random generation usage
grep -r "SecureRandom\|os.urandom\|crypto.getRandomBytes\|rand::thread_rng" . --include="*.py" --include="*.js" --include="*.rb" --include="*.rs" | head -20
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.
- Exact versions of cryptographic and bcrypt libraries are confirmed from lock file and their APIs verified against official documentation for the resolved versions.
- Generation function returns both plaintext token for immediate distribution and triggers hashing for storage.
- Verification function uses constant-time comparison via the hashing library's built-in compare function.

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-CRYPTO rules marked MUST are non-negotiable. Verification commands MUST be executed before any code using cryptographic or hashing libraries is written. Lock file discovery and version grounding are mandatory prerequisites.
</enforcement>