# Store API Key Credentials Using Salted Hash Functions: Generated Key Tokens

These rules are ALWAYS ACTIVE for all API key credential storage operations, token generation for new API keys, authentication verification flows that validate API keys, and database persistence of hashed credentials.

### Rules

- **R-CRED-001** MUST: Generated API key tokens MUST derive from cryptographically secure random bytes with minimum entropy of 128 bits.
- **R-CRED-002** MUST: API key tokens MUST be hashed using salted hash functions before database persistence.
- **R-CRED-003** MUST: Only hashed values MUST be persisted to the database schema; plaintext tokens MUST never be stored.
- **R-CRED-004** MUST: Token verification MUST use constant-time comparison to prevent timing-based attacks.
- **R-CRED-005** MUST: Cryptographic hash and comparison functions MUST be imported from resolved cryptographic dependencies declared in the project's dependency manifest.
- **R-CRED-006** SHOULD: Implement usage tracking logic that compares current timestamp against last-used timestamp to determine whether an update is necessary, avoiding unnecessary write operations on every authentication.
- **R-CRED-007** SHOULD: Configure hash function work factor to balance security strength against computational cost, with periodic re-evaluation as compute costs decline.

### Verify

```bash
# Discover the project's test suite location and execute the authentication module tests
# to verify hash generation and verification behavior.
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -5

# Locate the project's static analysis or linting configuration and run security-focused
# rules to detect plaintext credential storage patterns.
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'sonar-project.properties' \) | head -5

# Identify the project's dependency audit tooling and execute vulnerability scans
# against the resolved cryptographic library versions.
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \) | head -1
```

**Accept when:**
- All authentication module tests pass, including hash generation, verification with valid tokens, rejection of invalid tokens, and constant-time comparison behavior.
- Static analysis reports no plaintext credential storage violations and no use of deprecated or insecure cryptographic functions.
- Dependency audit shows no known vulnerabilities in cryptographic libraries at the resolved versions, or documented exceptions exist for accepted risks.
- Code review confirms that API key storage follows hashing requirements and that plaintext tokens are never persisted to any storage layer.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are non-negotiable. Violations block merge and deployment until resolved or formally excepted through the documented exception process.
</enforcement>