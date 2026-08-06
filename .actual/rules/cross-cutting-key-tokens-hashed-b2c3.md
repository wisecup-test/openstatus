# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Tokens Hashed

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all public contracts exposing key lifecycle management and authentication flows.

### Rules

- **R-CRYPTO-001** MUST: API key tokens MUST be hashed using bcrypt with a minimum cost factor of 10 before persistence.

### Verify

```bash
# Discover and execute test suites for API key generation, hashing, and verification
find . -type f -name '*test*' -o -name '*spec*' | grep -i 'key\|auth\|crypto' | head -20
# Execute the project's test runner for API key-related test suites
# (exact command depends on build tool discovered from manifest)

# Discover and execute static analysis or linting configuration
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' \) | head -5
# Run security-focused linting rules that detect plaintext credential storage or weak random generation

# Discover and execute dependency scanning tool
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \) | head -3
# Execute vulnerability checks against cryptographic and hashing libraries at resolved versions
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before approving changes to API key generation, hashing, or verification logic. CI pipeline failures block merge for test failures, static analysis violations, or dependency vulnerabilities in cryptographic libraries.
</enforcement>