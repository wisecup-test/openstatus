# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Token Verification Use

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files that handle token lifecycle management, authentication flows, and credential persistence.

### Rules

- **R-CRYPTO-001** MUST: Token verification MUST use constant-time comparison functions provided by the hashing library to prevent timing attacks.

### Verify

```bash
# Discover the project's test suite location and identify test files covering API key generation, hashing, and verification
# Execute the test runner for these specific test suites
find . -type f -name '*test*' -o -name '*spec*' | grep -i 'key\|auth\|token' | head -20

# Discover the project's static analysis or linting configuration
# Execute the security-focused linting rules that detect plaintext credential storage or weak random generation patterns
grep -r 'plaintext\|weak.*random\|credential' . --include='*.json' --include='*.yaml' --include='*.yml' --include='*.toml' 2>/dev/null | head -10

# Discover the project's dependency scanning tool and execute vulnerability checks
# against the cryptographic and hashing libraries at their resolved versions
find . -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' | head -5
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.
- Code review confirms constant-time comparison is delegated to the hashing library's built-in compare function rather than manual string comparison.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification conditions must pass before accepting implementation. Violations block merge in CI pipeline.
</enforcement>