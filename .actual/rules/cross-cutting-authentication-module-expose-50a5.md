# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Authentication Module Expose

These rules are ALWAYS ACTIVE for all authentication module code responsible for API key generation, hashing, verification, and usage tracking.

### Rules

- **R-AUTH-001** SHOULD: The authentication module SHOULD expose separate public contracts for key generation, hashing, verification, and usage tracking to maintain separation of concerns.

### Verify

```bash
# Locate and run authentication utility tests
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -5
# Execute test runner with credential management filters
# (Exact command depends on discovered test framework)

# Inspect CI configuration for security scanning
find . -type f \( -name '.github' -o -name '.gitlab-ci.yml' -o -name 'Jenkinsfile' -o -name '.circleci' \) | head -3

# Search for dependency manifest and lock files
find . -type f \( -name 'package.json' -o -name 'package-lock.json' -o -name 'requirements.txt' -o -name 'Gemfile' -o -name 'Gemfile.lock' -o -name 'go.mod' -o -name 'go.sum' -o -name 'pom.xml' -o -name 'build.gradle' \) | head -5

# Locate static analysis or linting configurations
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.rubocop.yml' -o -name 'golangci.yml' \) | head -3
```

**Accept when:**
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- API key generation uses platform-provided cryptographically secure random sources with documented entropy guarantees
- Stored API key hashes use adaptive hashing with configurable work factors (e.g., bcrypt)
- Key verification operations use constant-time comparison to prevent timing side-channels
- Separate public contracts exist for generation, hashing, verification, and usage tracking functions

<enforcement>
Claude Code MUST NOT skip or defer verification. All credential management tests, dependency scans, and static analysis checks MUST pass before accepting changes to authentication module code. Security review is mandatory for all authentication module modifications.
</enforcement>