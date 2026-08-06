# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Generation Use

These rules are ALWAYS ACTIVE for all API key generation, storage, verification, and credential management code within the authentication module.

### Rules

- **R-CRYPT-001** MUST: API key generation MUST use cryptographically secure random number generation with sufficient entropy to prevent prediction or enumeration attacks.

### Verify

```bash
# Locate the project's test suite directory structure and identify verification scripts for authentication utilities
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -20

# Execute the discovered test runner with appropriate filters for credential management tests
# (Adjust command based on discovered test framework)
grep -r "key.*generation\|api.*key" . --include="*test*" --include="*spec*" | head -20

# Inspect the project's continuous integration configuration to identify security scanning commands
find . -type f \( -name '.github' -o -name '.gitlab-ci.yml' -o -name 'Jenkinsfile' -o -name '.circleci' \) 2>/dev/null

# Search for dependency manifest files to identify cryptographic library versions
find . -type f \( -name 'package.json' -o -name 'Gemfile' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Cargo.toml' -o -name 'pom.xml' \) 2>/dev/null

# Search the repository for static analysis or linting configurations
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name 'clippy.toml' \) 2>/dev/null

# Verify no plaintext credential storage in authentication module
grep -r "password\|api.key\|secret" . --include="*.js" --include="*.py" --include="*.go" --include="*.rs" | grep -v "hash\|bcrypt\|crypto" | head -20
```

**Accept when:**
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- API key generation code exclusively uses platform-provided cryptographically secure random sources
- All stored API keys are verified to use adaptive hashing with configurable work factors

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication module changes. Violations block merge until remediated.
</enforcement>