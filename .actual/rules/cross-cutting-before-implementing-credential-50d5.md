# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Before Implementing Credential

These rules are ALWAYS ACTIVE for all public API authentication implementations, credential generation and management workflows, and API key lifecycle operations including creation, verification, and revocation.

### Rules

- **R-CRED-001** MUST: Before implementing credential hashing, discover the project's dependency lock artifact, resolve the exact installed version of the adaptive hashing library, and verify API compatibility with that version's official documentation.
- **R-CRED-002** MUST: Generate API keys with minimum 128-bit entropy from cryptographic random sources; encode in URL-safe format for transmission and storage in HTTP headers.
- **R-CRED-003** MUST: Store only irreversible hashes of API keys rather than plaintext tokens to protect against database compromise.
- **R-CRED-004** SHOULD: Configure adaptive hashing work factors based on authentication latency requirements; target 100-250ms per hash operation as baseline for acceptable user experience.
- **R-CRED-005** SHOULD: Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication; consider caching last-used timestamps with eventual consistency.
- **R-CRED-006** MUST: Enforce transport-layer security for all API endpoints and implement detection for plaintext credential transmission attempts in monitoring systems.
- **R-CRED-007** SHOULD: Implement expiration metadata in credential storage and automated notification workflows for approaching expiration dates.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite
# covering credential generation, hashing, and verification functions

# Locate the project's static analysis or linting configuration and execute
# security-focused rules checking for plaintext credential storage

# Identify the project's dependency scanning tooling and verify no known
# vulnerabilities exist in cryptographic or hashing dependencies
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries
- Exact version of the adaptive hashing library is documented and verified against official documentation

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked. All credential lifecycle functions must pass security-focused testing before merge.
</enforcement>