# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Tokens Generated

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files in the authentication and credential management subsystems.

### Rules

- **R-KEYTOKEN-001** MUST: API key tokens MUST be generated using cryptographically secure random byte generation with minimum 128-bit entropy.

### Verify

```bash
# Discover the project's test suite location and identify test files covering API key generation, hashing, and verification
find . -type f -name '*test*' -o -name '*spec*' | grep -i 'key\|auth\|token' | head -20

# Execute the test runner for API key generation, hashing, and verification test suites
# (Exact command depends on build tool discovered from manifest)

# Discover the project's static analysis or linting configuration
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'sonar-project.properties' \) | head -5

# Execute security-focused linting rules that detect plaintext credential storage or weak random generation patterns
# (Exact command depends on linting tool discovered)

# Discover the project's dependency scanning tool and execute vulnerability checks
# (Exact command depends on build tool and security scanning tool discovered)
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before approving changes to API key generation, hashing, or verification logic.
</enforcement>