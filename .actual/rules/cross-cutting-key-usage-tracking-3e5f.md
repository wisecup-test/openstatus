# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Key Usage Tracking

These rules are ALWAYS ACTIVE for all public API authentication code, service-to-service authentication mechanisms, client credential generation and management workflows, and API key lifecycle operations including creation, verification, and revocation.

### Rules

- **R-CRYPTO-001** MUST: Generate API keys with minimum 128-bit entropy from cryptographic random sources; encode in URL-safe format for transmission and storage in HTTP headers.
- **R-CRYPTO-002** MUST: Store API keys using adaptive hashing (bcrypt or equivalent) rather than plaintext or reversible encryption.
- **R-CRYPTO-003** MUST: Configure adaptive hashing work factors based on authentication latency requirements; target 100-250ms per hash operation as baseline for acceptable user experience.
- **R-CRYPTO-004** SHOULD: API key usage tracking should implement rate-limiting logic to determine when last-used timestamps require updates.
- **R-CRYPTO-005** SHOULD: Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication; consider caching last-used timestamps with eventual consistency.
- **R-CRYPTO-006** MUST: Enforce transport-layer security for all API endpoints and implement detection for plaintext credential transmission attempts in monitoring systems.
- **R-CRYPTO-007** SHOULD: Implement expiration metadata in credential storage and automated notification workflows for approaching expiration dates.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite covering credential generation, hashing, and verification functions
# (Exact command depends on project build tool — consult dependency manifest and lock file)

# Locate the project's static analysis or linting configuration and execute security-focused rules checking for plaintext credential storage
# (Exact command depends on project security tooling — consult project configuration)

# Identify the project's dependency scanning tooling and verify no known vulnerabilities exist in cryptographic or hashing dependencies
# (Exact command depends on project dependency scanning tool — consult project configuration)
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries
- Work factor configuration is documented and baseline performance testing confirms authentication latency remains within acceptable bounds

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked by CI checks. All credential lifecycle functions must pass security-focused testing before merge.
</enforcement>