# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Key Verification Use

These rules are ALWAYS ACTIVE for all code implementing API key authentication, credential storage, and verification within the authentication layer.

### Rules

- **R-CRYPTO-001** MUST: API key verification MUST use constant-time comparison functions provided by the hashing library to prevent timing attacks.

### Verify

```bash
# Discover the project's test suite location and execute tests covering API key generation, hashing, verification, and timing attack resistance.
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(auth|key|crypto)' | head -20

# Locate the project's security scanning configuration and run dependency vulnerability checks against all cryptographic libraries.
if [ -f 'package.json' ]; then npm audit; fi
if [ -f 'Gemfile.lock' ]; then bundle audit; fi
if [ -f 'go.mod' ]; then go list -json -m all | nancy sleuth; fi
if [ -f 'requirements.txt' ] || [ -f 'Pipfile' ]; then pip-audit; fi

# Identify the project's benchmarking tools and measure authentication latency under load to validate cost factor configuration.
grep -r 'benchmark\|perf\|load' . --include='*.json' --include='*.yaml' --include='*.yml' --include='*.toml' | head -10
```

**Accept when:**
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.
- Code review confirms constant-time comparison is used for all API key verification operations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All cryptographic library usage must be validated against the exact resolved version in the project's lock file before implementation. Timing attack resistance tests must pass before any code is committed.
</enforcement>