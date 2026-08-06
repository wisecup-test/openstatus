# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Keys Not Stored

These rules are ALWAYS ACTIVE for all API key generation, storage, persistence, verification, and credential management code within the authentication module.

### Rules

- **R-KEYS-001** MUST: API keys MUST NOT be stored in plaintext; only cryptographic hashes of keys MUST be persisted.

### Verify

```bash
# Locate the project's test suite directory structure and identify verification scripts for authentication utilities
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -20

# Execute the discovered test runner with appropriate filters for credential management tests
# (Adjust command based on discovered test framework)
grep -r "key.*generat\|hash.*verif\|timing.*attack" . --include="*.md" --include="*.txt" | head -10

# Inspect the project's continuous integration configuration to identify security scanning commands
find . -type f \( -name '.github' -o -name '.gitlab-ci.yml' -o -name 'Jenkinsfile' -o -name '.circleci' \) 2>/dev/null

# Search the repository for static analysis or linting configurations
find . -type f \( -name '.eslintrc*' -o -name 'sonar-project.properties' -o -name '.bandit' -o -name 'pylintrc' \) 2>/dev/null

# Locate the dependency manifest to identify cryptographic libraries
find . -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' -o -name 'go.mod' -o -name 'pom.xml' -o -name 'Cargo.toml' \) 2>/dev/null

# Search for plaintext credential storage patterns
grep -r "password.*=\|api.key.*=\|secret.*=" . --include="*.js" --include="*.py" --include="*.go" --include="*.java" 2>/dev/null | grep -v "hash\|bcrypt\|crypto" | head -20

# Verify cryptographic random generation usage
grep -r "crypto.random\|SecureRandom\|os.urandom\|rand.Reader" . --include="*.js" --include="*.py" --include="*.go" --include="*.java" 2>/dev/null
```

**Accept when:**
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- Code review confirms API keys are only persisted as cryptographic hashes, never in plaintext or reversible form
- Work factor configuration for adaptive hashing is documented and baselined through performance testing

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for any code changes affecting API key generation, hashing, storage, or verification. Violations block merge until remediated.
</enforcement>