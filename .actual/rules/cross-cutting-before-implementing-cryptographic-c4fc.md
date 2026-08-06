# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Before Implementing Cryptographic

These rules are ALWAYS ACTIVE for all authentication module code implementing API key generation, storage, verification, and credential management utilities.

### Rules

- **R-CRYPTO-001** MUST: Before implementing cryptographic operations with any versioned library, determine the exact resolved version from the project's lock or resolution artifact.
- **R-CRYPTO-002** MUST: Verify the exact resolved version against official documentation, changelog, or public API reference for that specific version before using any cryptographic API, class, or function.
- **R-CRYPTO-003** MUST: Use cryptographically secure random generation for API key creation to prevent enumeration and prediction attacks.
- **R-CRYPTO-004** MUST: Use adaptive hashing with configurable work factors for storing API key hashes to resist brute-force attacks.
- **R-CRYPTO-005** MUST: Implement constant-time comparison operations during API key verification to eliminate timing side-channels.
- **R-CRYPTO-006** SHOULD: Establish work factor baselines through load testing that measures authentication latency under expected concurrent request volumes, targeting sub-second response times.
- **R-CRYPTO-007** SHOULD: Implement key rotation mechanisms supporting both active and deprecated keys during transition periods.
- **R-CRYPTO-008** SHOULD: Implement rate limiting and anomaly detection on API key usage patterns to detect compromised credentials.

### Verify

```bash
# 1. Locate dependency manifest and identify build tool
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Gemfile' -o -name 'go.mod' -o -name 'Cargo.toml' \) | head -1

# 2. Inspect lock/resolution artifact for exact cryptographic library versions
find . -maxdepth 2 -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pom.lock' -o -name 'gradle.lock' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' \) | head -1

# 3. Locate and execute authentication test suite
find . -type f -path '*/test*' \( -name '*auth*' -o -name '*credential*' -o -name '*key*' \) | grep -E '\.(test\.|_test\.|spec\.)' | head -5

# 4. Execute test runner with credential management filters
find . -maxdepth 2 -type f \( -name 'pytest.ini' -o -name 'jest.config.*' -o -name 'mocha.opts' -o -name '.rspec' \) | head -1

# 5. Locate CI configuration for security scanning
find . -maxdepth 2 -type f \( -name '.github/workflows/*.yml' -o -name '.gitlab-ci.yml' -o -name '.circleci/config.yml' -o -name 'Jenkinsfile' \) | head -1

# 6. Search for static analysis or linting configurations
find . -maxdepth 2 -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'sonar-project.properties' \) | head -1

# 7. Verify no plaintext credential storage in codebase
grep -r "api.?key\|password\|secret" . --include="*.py" --include="*.js" --include="*.java" --include="*.go" | grep -v node_modules | grep -v '.git' | grep -E "=\s*['\"]" | head -10

# 8. Confirm cryptographically secure random generation usage
grep -r "crypto\.random\|SecureRandom\|secrets\.token\|rand\.Reader" . --include="*.py" --include="*.js" --include="*.java" --include="*.go" | grep -v node_modules | grep -v '.git' | head -10
```

**Accept when:**
- Exact resolved versions of all cryptographic libraries are documented from lock/resolution artifacts
- Official documentation for each exact version is consulted and confirmed to contain all used APIs
- All credential management tests pass, including generation uniqueness, hash verification, and timing attack resistance tests
- Dependency security scans report no known vulnerabilities in cryptographic libraries at severity medium or above
- Static analysis confirms no plaintext credential storage and all random generation uses cryptographically secure sources
- Work factor baseline testing results are documented with authentication latency measurements
- Key rotation and rate limiting mechanisms are implemented and tested

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-CRYPTO rules marked MUST are non-negotiable before any cryptographic implementation proceeds. Lock-version grounding (R-CRYPTO-001 and R-CRYPTO-002) MUST be completed before writing any code that uses versioned cryptographic libraries.
</enforcement>