# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Key Tokens Hashed

These rules are ALWAYS ACTIVE for all public API authentication code paths, API key generation and verification functions, credential storage mechanisms, and service-to-service authentication implementations.

### Rules

- **R-CRYPT-001** MUST: API key tokens must be hashed using adaptive key derivation functions with configurable work factors before storage.
- **R-CRYPT-002** MUST: Generate API keys with minimum 128-bit entropy from cryptographic random sources.
- **R-CRYPT-003** MUST: Encode API keys in URL-safe format for transmission and storage in HTTP headers.
- **R-CRYPT-004** MUST: Enforce transport-layer security for all API endpoints requiring authentication.
- **R-CRYPT-005** SHOULD: Configure adaptive hashing work factors based on authentication latency requirements; target 100-250ms per hash operation as baseline.
- **R-CRYPT-006** SHOULD: Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication.
- **R-CRYPT-007** SHOULD: Consider caching last-used timestamps with eventual consistency for usage tracking.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite
# covering credential generation, hashing, and verification functions
./run-tests.sh --filter="credential|api_key|hash|verify"

# Locate the project's static analysis or linting configuration and execute
# security-focused rules checking for plaintext credential storage
./run-lint.sh --security-rules

# Identify the project's dependency scanning tooling and verify no known
# vulnerabilities exist in cryptographic or hashing dependencies
./scan-dependencies.sh --check-crypto --check-hashing
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries
- Work factor configuration is documented and baseline performance testing confirms authentication latency remains within acceptable bounds

<enforcement>
Clause MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked by CI checks. All credential lifecycle functions require explicit verification before merge.
</enforcement>