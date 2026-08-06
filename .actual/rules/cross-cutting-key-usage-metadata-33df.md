# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Usage Metadata

These rules are ALWAYS ACTIVE for all authentication module code handling API key generation, storage, verification, and usage tracking.

### Rules

- **R-CRYPT-001** SHOULD: API key usage metadata SHOULD be tracked to support security auditing and anomaly detection.

### Verify

```bash
# Locate the project's test suite directory structure and identify verification scripts for authentication utilities
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -20

# Execute the discovered test runner with appropriate filters for credential management tests
# (Exact command depends on build tool discovered from manifest)

# Inspect the project's continuous integration configuration to identify security scanning commands
find . -type f \( -name '.github' -o -name '.gitlab-ci.yml' -o -name 'Jenkinsfile' -o -name '.circleci' \) | head -10

# Search the repository for static analysis or linting configurations
find . -type f \( -name '.eslintrc*' -o -name 'sonar-project.properties' -o -name '.bandit' -o -name 'pylintrc' \) | head -10

# Verify no plaintext credential storage in authentication module
grep -r "password\|secret\|key" . --include="*.js" --include="*.ts" --include="*.py" --include="*.java" | grep -v "hash\|bcrypt\|crypto" | head -20

# Verify cryptographically secure random generation is used
grep -r "crypto\|SecureRandom\|urandom" . --include="*.js" --include="*.ts" --include="*.py" --include="*.java" | grep -i "random\|generate" | head -20
```

**Accept when:**
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- API key usage tracking is implemented and verified in authentication flow tests
- Code review confirms usage metadata is captured for security auditing purposes

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication module changes.
</enforcement>