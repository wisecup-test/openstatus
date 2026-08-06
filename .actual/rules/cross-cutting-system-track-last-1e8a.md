# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: System Track Last

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files in the authentication and credential management subsystems.

### Rules

- **R-CRYPTO-001** MUST: Use cryptographic random generation (minimum 128-bit entropy) for all API key token creation.
- **R-CRYPTO-002** MUST: Hash all API keys using bcrypt before storage; never store plaintext credentials in persistent layers.
- **R-CRYPTO-003** MUST: Use constant-time comparison (delegated to bcrypt library's built-in compare function) during API key verification to prevent timing-based side-channel attacks.
- **R-CRYPTO-004** MUST: Implement generation, hashing, and verification as separate, testable units with clear public contracts.
- **R-CRYPTO-005** MUST: Never log or transmit plaintext API key tokens after initial generation response.
- **R-CRYPTO-006** MAY: Track last-used timestamps during verification to support usage analytics and key rotation policies.
- **R-CRYPTO-007** MUST: Verify exact cryptographic and bcrypt library versions from the project's lock file before implementation; do not rely on training-data recall.
- **R-CRYPTO-008** MUST: Confirm all cryptographic API signatures, cost factor parameters, and encoding options exist in the resolved library version's official documentation.
- **R-CRYPTO-009** MUST: Design the verification function to return both authentication success status and metadata for usage tracking.

### Verify

```bash
# Discover and execute the project's test suite for API key generation, hashing, and verification
find . -type f -name '*test*' | grep -E '(key|auth|crypto)' | head -20
# Execute test runner for API key-related test suites
# (exact command depends on project's build tool discovered from manifest)

# Discover and execute static analysis or linting configuration
find . -type f -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'sonar-project.properties'
# Run security-focused linting rules detecting plaintext credential storage or weak random generation

# Discover the project's dependency scanning tool and lock file
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' \)
# Execute vulnerability checks against cryptographic and hashing libraries at resolved versions
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.
- Code review verification confirms cryptographic library usage matches the exact resolved version's API and hash parameters are correctly configured.

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-CRYPTO rules marked MUST are mandatory and must be verified before code is committed. Violations block merge in the CI pipeline.
</enforcement>