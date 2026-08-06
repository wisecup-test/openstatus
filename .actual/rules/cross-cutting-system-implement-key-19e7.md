# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: System Implement Key

These rules are ALWAYS ACTIVE for all authentication module code, credential management utilities, and API key generation, storage, and verification implementations.

### Rules

- **R-CRYPT-001** MUST: Use cryptographically secure random generation for all API key creation to prevent enumeration and prediction attacks.
- **R-CRYPT-002** MUST: Store API key hashes using adaptive hashing algorithms with configurable work factors; never store keys in plaintext or using reversible encryption.
- **R-CRYPT-003** MUST: Implement constant-time comparison operations during API key verification to eliminate timing side-channels.
- **R-CRYPT-004** MUST: Maintain separation of concerns through distinct public contracts for key generation, hashing, verification, and usage tracking.
- **R-CRYPT-005** SHOULD: Establish work factor baselines through load testing that measures authentication latency under expected concurrent request volumes, targeting sub-second response times while maximizing computational cost.
- **R-CRYPT-006** SHOULD: Implement key rotation mechanisms that allow graceful migration from old to new keys without service disruption, supporting both active and deprecated keys during transition periods.
- **R-CRYPT-007** MAY: Implement key rotation policies based on usage patterns or time-based expiration.
- **R-CRYPT-008** SHOULD: Consider implementing rate limiting and anomaly detection on API key usage patterns to detect compromised credentials before significant damage occurs.

### Verify

```bash
# Locate and run credential management tests
find . -type f -name '*test*' -o -name '*spec*' | grep -i 'auth\|credential\|key' | head -5
# Execute test runner with authentication utility filters (exact command depends on discovered test framework)

# Inspect CI configuration for security scanning commands
find . -type f \( -name '.github' -o -name '.gitlab-ci.yml' -o -name 'Jenkinsfile' -o -name '.circleci' \) -o -name '*security*' | head -10

# Search for static analysis or linting configurations
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'sonar-project.properties' \) | head -5

# Locate dependency manifest and lock files
find . -type f \( -name 'package.json' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'Gemfile' -o -name 'Gemfile.lock' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'go.sum' -o -name 'pom.xml' -o -name 'build.gradle' \) | head -10

# Verify no plaintext credential storage in codebase
grep -r "api.?key\|password\|secret" . --include="*.js" --include="*.py" --include="*.java" --include="*.go" | grep -v "hash\|bcrypt\|encrypt" | head -20
```

**Accept when:**
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- Load testing confirms authentication latency remains sub-second under expected concurrent request volumes
- Code review confirms separation of concerns across key generation, hashing, verification, and usage tracking contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are non-negotiable; SHOULD rules require documented justification if deferred; MAY rules are optional but recommended. Verification commands MUST be executed before approving authentication module changes. Build failures from test or static analysis violations MUST block merge. Security scan findings MUST trigger immediate review and remediation workflow.
</enforcement>