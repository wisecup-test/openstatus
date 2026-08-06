# Store API Key Credentials Using Salted Hash Functions: Plaintext Key Tokens

These rules are ALWAYS ACTIVE for all API key credential storage operations, token generation for new API keys, authentication verification flows that validate API keys, and database persistence of hashed credentials.

### Rules

- **R-CRED-001** MUST_NOT: Plaintext API key tokens MUST_NOT be persisted in any storage layer after initial generation and presentation to the user.

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
find . -type f \( -name 'package-lock.json' -o -name 'Pipfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' \) | head -1
```

**Accept when:**
- All authentication module tests pass, including hash generation, verification with valid tokens, rejection of invalid tokens, and constant-time comparison behavior.
- Static analysis reports no plaintext credential storage violations and no use of deprecated or insecure cryptographic functions.
- Dependency audit shows no known vulnerabilities in cryptographic libraries at the resolved versions, or documented exceptions exist for accepted risks.

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria MUST be confirmed before approving any code that implements or modifies API key credential storage.
</enforcement>