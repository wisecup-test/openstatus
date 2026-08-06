# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Hashing Use

These rules are ALWAYS ACTIVE for all API key generation, storage, verification, and credential management code within the authentication module.

### Rules

- **R-CRYPT-001** MUST: API key hashing MUST use adaptive hashing algorithms with configurable work factors to resist brute-force and rainbow table attacks.

### Verify

```bash
# Locate the project's test suite directory structure and identify verification scripts for authentication utilities
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -20

# Execute the discovered test runner with appropriate filters for credential management tests
# (Exact command depends on project's build tool — inspect package.json, pom.xml, build.gradle, or pyproject.toml)

# Inspect the project's continuous integration configuration to identify security scanning and dependency audit commands
find . -type f \( -name '.github' -o -name '.gitlab-ci.yml' -o -name 'Jenkinsfile' -o -name '.circleci' \) | head -10

# Search the repository for static analysis or linting configurations that enforce secure coding patterns
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name 'checkstyle.xml' -o -name 'sonar-project.properties' \) | head -10

# Verify no plaintext credential storage in authentication module
grep -r "password\|secret\|key" . --include="*.js" --include="*.ts" --include="*.py" --include="*.java" | grep -v "hash\|bcrypt\|crypto" | head -20

# Verify cryptographically secure random generation is used
grep -r "crypto\.random\|SecureRandom\|secrets\.token\|os\.urandom" . --include="*.js" --include="*.ts" --include="*.py" --include="*.java" | head -20
```

**Accept when:**
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- API key hashing implementation uses adaptive algorithms (bcrypt, scrypt, argon2, or equivalent) with configurable work factors
- Code review confirms separation of concerns between key generation, hashing, and verification functions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication module changes.
</enforcement>