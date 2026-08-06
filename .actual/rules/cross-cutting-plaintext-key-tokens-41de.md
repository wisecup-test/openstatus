# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Plaintext Key Tokens

These rules are ALWAYS ACTIVE for all public API authentication code, service-to-service authentication mechanisms, and client credential generation and management workflows.

### Rules

- **R-CRYPTO-001** MUST_NOT: Plaintext API key tokens must not be persisted in any storage system after initial generation and presentation to the user.
- **R-CRYPTO-002** MUST: Generate API keys with minimum 128-bit entropy from cryptographic random sources; encode in URL-safe format for transmission and storage in HTTP headers.
- **R-CRYPTO-003** MUST: Use adaptive hashing with configurable work factors for credential storage; target 100-250ms per hash operation as baseline for acceptable user experience.
- **R-CRYPTO-004** MUST: Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication; consider caching last-used timestamps with eventual consistency.
- **R-CRYPTO-005** MUST: Enforce transport-layer security for all API endpoints and implement detection for plaintext credential transmission attempts in monitoring systems.
- **R-CRYPTO-006** SHOULD: Implement expiration metadata in credential storage and automated notification workflows for approaching expiration dates.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite covering credential generation, hashing, and verification functions
# (Exact command depends on project build tool — consult dependency manifest and lock file)

# Locate the project's static analysis or linting configuration and execute security-focused rules checking for plaintext credential storage
# (Exact command depends on project static analysis tooling — consult project configuration)

# Identify the project's dependency scanning tooling and verify no known vulnerabilities exist in cryptographic or hashing dependencies
# (Exact command depends on project dependency scanning tool — consult project configuration)
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries
- No plaintext API key tokens are found persisted in any storage system
- All API endpoints enforce transport-layer security

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked by CI checks. All rules in this file are mandatory and must be verified before acceptance.
</enforcement>