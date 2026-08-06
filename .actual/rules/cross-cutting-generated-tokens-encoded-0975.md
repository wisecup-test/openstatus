# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Generated Tokens Encoded

These rules are ALWAYS ACTIVE for all API key generation, storage, and verification operations within the system, including all files in the authentication and credential management subsystems.

### Rules

- **R-CRYPT-001** MUST: Generated tokens MUST be encoded in a URL-safe format suitable for HTTP header transmission.

### Verify

```bash
# Discover the project's test suite location and identify test files covering API key generation, hashing, and verification
find . -type f -name '*test*.py' -o -name '*_test.py' | grep -i 'key\|auth\|crypt' | head -20

# Execute the test runner for API key generation, hashing, and verification test suites
python -m pytest -v --tb=short -k 'api_key or token or hash or verify' 2>&1 | tee /tmp/test_results.log

# Discover the project's static analysis or linting configuration
find . -maxdepth 2 -type f \( -name '.pylintrc' -o -name 'pyproject.toml' -o -name 'setup.cfg' -o -name '.flake8' \)

# Execute security-focused linting rules that detect plaintext credential storage or weak random generation
pylint --load-plugins=pylint_security . 2>&1 | grep -i 'plaintext\|credential\|random\|secret' || echo 'No security issues detected'

# Discover the project's dependency scanning tool and execute vulnerability checks
if command -v safety &> /dev/null; then safety check --json; fi
if command -v pip-audit &> /dev/null; then pip-audit; fi

# Verify cryptographic library is available and provides secure random generation
python -c "from secrets import token_urlsafe; print('URL-safe token generation available')"

# Verify bcrypt library is available and provides hash/compare functions
python -c "import bcrypt; print('bcrypt version:', bcrypt.__version__); print('hash and compare functions available')"
```

**Accept when:**
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.
- Cryptographic library provides `token_urlsafe()` or equivalent URL-safe encoding function.
- Bcrypt library provides both hash generation and constant-time comparison functions with configurable cost factor.

<enforcement>
Claude Code MUST NOT skip or defer verification. All verification commands MUST execute successfully before accepting implementation. Test failures, static analysis violations, or dependency vulnerabilities MUST block acceptance.
</enforcement>