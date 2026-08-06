# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: System Implement Rate

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files in the authentication and credential management subsystems.

### Rules

- **R-CRYPTO-001** MUST: Implement cryptographic random generation for all API key creation operations, using the project's cryptographic library's secure random byte generation function with minimum 128-bit entropy.
- **R-CRYPTO-002** MUST: Hash all API keys before storage using bcrypt with an adaptive cost factor, never storing plaintext credentials in persistent layers.
- **R-CRYPTO-003** MUST: Implement constant-time comparison during API key verification by delegating to the hashing library's built-in compare function rather than manual string comparison.
- **R-CRYPTO-004** SHOULD: Implement rate limiting on verification attempts to mitigate brute-force attacks against stored hashes.
- **R-CRYPTO-005** MUST: Separate API key generation, hashing, and verification into distinct, testable units with clear contracts.
- **R-CRYPTO-006** MUST: Never log or transmit plaintext tokens after initial generation response.
- **R-CRYPTO-007** MUST: Design the verification function to return both authentication success status and metadata for usage tracking.
- **R-CRYPTO-008** SHOULD: Establish annual review cycle for bcrypt cost factor adequacy to maintain minimum 100ms verification time on current hardware.
- **R-CRYPTO-009** SHOULD: Implement hash upgrade mechanism that transparently re-hashes credentials with higher cost on next successful authentication.
- **R-CRYPTO-010** SHOULD: Monitor authentication latency and CPU utilization to detect potential denial of service from bcrypt computational cost.

### Verify

```bash
# Discover the project's test suite location and execute API key tests
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(key|auth|crypto)' | head -20

# Execute the test runner for API key generation, hashing, and verification suites
# (Exact command depends on project's build tool discovered from manifest)

# Discover the project's static analysis or linting configuration
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'sonar-project.properties' \)

# Execute security-focused linting rules
# (Exact command depends on project's linting tool)

# Discover the project's dependency scanning tool
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \)

# Execute vulnerability checks against cryptographic and hashing libraries
# (Exact command depends on project's dependency scanner)

# Verify no plaintext API key storage in persistent layers
grep -r "api.?key" . --include="*.js" --include="*.ts" --include="*.py" --include="*.java" | grep -v "hash" | grep -v "bcrypt" | grep -v "test"

# Verify cryptographic random generation is used
grep -r "crypto.*random\|randomBytes\|secure.*random" . --include="*.js" --include="*.ts" --include="*.py" --include="*.java"
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.
- Code review verification confirms cryptographic library usage matches the exact resolved version's API and hash parameters are correctly configured.
- Runtime monitoring is configured to alert on authentication failures exceeding threshold rates.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting implementation. Rules marked SHOULD represent strong recommendations that should be implemented unless documented exceptions exist with security team approval.
</enforcement>