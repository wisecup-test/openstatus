# Store API Key Credentials Using Salted Hash Functions: Key Tokens Hashed

These rules are ALWAYS ACTIVE for all API key credential storage operations, token generation for new API keys, authentication verification flows that validate API keys, and database persistence of hashed credentials.

### Rules

- **R-CRED-001** MUST: API key tokens MUST be hashed using a salted cryptographic hash function with a work factor of at least 10 rounds before storage.

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
find . -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Pipfile.lock' -o -name 'go.sum' \) | head -3
```

**Accept when:**
- All authentication module tests pass, including hash generation, verification with valid tokens, rejection of invalid tokens, and constant-time comparison behavior.
- Static analysis reports no plaintext credential storage violations and no use of deprecated or insecure cryptographic functions.
- Dependency audit shows no known vulnerabilities in cryptographic libraries at the resolved versions, or documented exceptions exist for accepted risks.
- Generated tokens are presented to the user exactly once during creation and only the hashed value is persisted to the database schema.
- Usage tracking logic compares current timestamp against last-used timestamp to determine whether an update is necessary, avoiding unnecessary write operations on every authentication.

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication module tests MUST pass, static analysis MUST report no violations, and dependency audits MUST show no unaccepted vulnerabilities before accepting changes that affect API key credential storage.
</enforcement>