# Store API Key Credentials Using Salted Hash Functions: System Expose Separate

These rules are ALWAYS ACTIVE for all API key credential storage operations, token generation for new API keys, authentication verification flows that validate API keys, and database persistence of hashed credentials.

### Rules

- **R-CRED-001** MAY: The system MAY expose separate public contracts for key generation, hashing, verification, and usage policy to enable modular testing and replacement.
- **R-CRED-002** MUST: API keys must be stored securely in the database using salted hash functions such that compromise of the storage layer does not expose plaintext credentials.
- **R-CRED-003** MUST: Token generation for new API keys must use cryptographic random byte generation with sufficient entropy (minimum 16 bytes).
- **R-CRED-004** MUST: Authentication verification flows must use constant-time comparison functions to prevent timing-based attacks that could leak information about stored hashes.
- **R-CRED-005** MUST: Only hashed values must be persisted to the database schema; plaintext tokens must be presented to the user exactly once during creation and never stored.
- **R-CRED-006** SHOULD: Usage tracking logic should compare current timestamp against last-used timestamp to determine whether an update is necessary, avoiding unnecessary write operations on every authentication.
- **R-CRED-007** MUST: Cryptographic dependencies must be pinned to exact versions in lock files and monitored for security vulnerabilities.
- **R-CRED-008** SHOULD: Work factor configuration should be benchmarked against current hardware capabilities and monitored for alignment with cryptographic best practices.
- **R-CRED-009** MUST: Rate limiting must be implemented on authentication endpoints to mitigate denial-of-service attacks through repeated authentication attempts.

### Verify

```bash
# Discover the project's test suite location and execute authentication module tests
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -5

# Execute authentication module tests to verify hash generation and verification behavior
# (Exact command depends on project's test runner — inspect package.json, Makefile, or build config)

# Locate the project's static analysis or linting configuration
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' \) | head -5

# Run security-focused linting rules to detect plaintext credential storage patterns
# (Exact command depends on project's linter — e.g., eslint, pylint, golangci-lint)

# Identify the project's dependency audit tooling
ls -la | grep -E 'package-lock.json|Pipfile.lock|go.sum|Cargo.lock'

# Execute vulnerability scans against resolved cryptographic library versions
# (Exact command depends on build tool — e.g., npm audit, pip-audit, cargo audit)

# Verify that generated tokens are presented to user exactly once and only hashed value is persisted
grep -r 'plaintext\|token.*store\|store.*token' --include='*.js' --include='*.py' --include='*.go' --include='*.ts' | grep -v test | grep -v node_modules

# Verify constant-time comparison is used for hash verification
grep -r 'timingAttack\|constantTime\|timing.*safe\|secure.*compare' --include='*.js' --include='*.py' --include='*.go' --include='*.ts' | head -10
```

**Accept when:**
- All authentication module tests pass, including hash generation, verification with valid tokens, rejection of invalid tokens, and constant-time comparison behavior.
- Static analysis reports no plaintext credential storage violations and no use of deprecated or insecure cryptographic functions.
- Dependency audit shows no known vulnerabilities in cryptographic libraries at the resolved versions, or documented exceptions exist for accepted risks.
- Code review confirms that API key storage follows hashing requirements and that plaintext tokens are never persisted to the database.
- Rate limiting is configured and verified on authentication endpoints.
- Work factor configuration is documented and benchmarked against current hardware capabilities.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for API key credential storage. Violations must be identified during code review and CI pipeline execution before merge.
</enforcement>