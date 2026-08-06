# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Plaintext Key Tokens

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files that implement or interact with API key lifecycle management, authentication flows, and credential persistence.

### Rules

- **R-APIKEY-001** MUST_NOT: Plaintext API key tokens MUST_NOT be stored in any persistent storage system after initial generation.
- **R-APIKEY-002** MUST: API key generation operations MUST use cryptographic random generation with minimum 128-bit entropy.
- **R-APIKEY-003** MUST: API keys MUST be hashed using bcrypt before storage in any persistent layer.
- **R-APIKEY-004** MUST: API key verification MUST use constant-time comparison by delegating to the hashing library's built-in compare function.
- **R-APIKEY-005** MUST: Generation, hashing, and verification functions MUST be implemented as separate, testable units with clear contracts.
- **R-APIKEY-006** MUST_NOT: Plaintext tokens MUST_NOT be logged or transmitted after the initial generation response.
- **R-APIKEY-007** MUST: The generation function MUST return both the plaintext token for immediate distribution and trigger hashing for storage.
- **R-APIKEY-008** MUST: Verification functions MUST return both authentication success status and metadata for usage tracking.
- **R-APIKEY-009** MUST: Bcrypt cost factor MUST be reviewed annually for adequacy against current hardware capabilities.
- **R-APIKEY-010** SHOULD: Implement hash upgrade mechanism that transparently re-hashes credentials with higher cost on next successful authentication.

### Verify

```bash
# Discover the project's test suite location and identify test files covering API key generation, hashing, and verification
find . -type f -name "*test*" -o -name "*spec*" | grep -i "key\|auth\|credential" | head -20

# Execute the test runner for API key-related test suites
# (Exact command depends on build tool discovered from manifest)
echo "Run: <build-tool> test --filter=api-key"

# Discover the project's static analysis or linting configuration
find . -type f \( -name ".eslintrc*" -o -name "pylintrc" -o -name ".flake8" -o -name "sonar-project.properties" \) | head -5

# Execute security-focused linting rules
echo "Run: <linter> --security-rules <source-directory>"

# Discover the project's dependency scanning tool
find . -type f \( -name "package-lock.json" -o -name "poetry.lock" -o -name "Gemfile.lock" -o -name "go.sum" \) | head -1

# Execute vulnerability checks against cryptographic and hashing libraries
echo "Run: <dependency-scanner> audit --severity=high"

# Verify no plaintext API key storage in persistent layers
grep -r "api.key\|apiKey\|API_KEY" . --include="*.py" --include="*.js" --include="*.ts" --include="*.java" | grep -v "hash\|bcrypt\|encrypt" | head -10

# Verify cryptographic random generation is used
grep -r "crypto.random\|secrets.token\|SecureRandom\|os.urandom" . --include="*.py" --include="*.js" --include="*.ts" --include="*.java" | grep -i "key\|token" | head -10
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.
- Code review confirms generation, hashing, and verification functions are implemented as separate units with clear contracts.
- Verification confirms bcrypt cost factor is documented and reviewed annually.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST_NOT are blocking violations. Violations in API key generation, storage, or verification logic require security team approval and incident response procedures.
</enforcement>