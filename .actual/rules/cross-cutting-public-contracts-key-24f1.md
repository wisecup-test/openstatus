# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Public Contracts Key

These rules are ALWAYS ACTIVE for all public API authentication code paths, credential generation and storage implementations, and service-to-service authentication mechanisms.

### Rules

- **R-PUBKEY-001** MUST: Public API contracts for key generation, hashing, verification, and usage tracking must be exported as distinct functions with clear separation of concerns.
- **R-PUBKEY-002** MUST: Generate API keys with minimum 128-bit entropy from cryptographic random sources; encode in URL-safe format for transmission and storage in HTTP headers.
- **R-PUBKEY-003** MUST: Store API keys using adaptive hashing with configurable work factors; never store plaintext credentials in any persistent storage.
- **R-PUBKEY-004** MUST: Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication; consider caching last-used timestamps with eventual consistency.
- **R-PUBKEY-005** SHOULD: Configure adaptive hashing work factors based on authentication latency requirements; target 100-250ms per hash operation as baseline for acceptable user experience.
- **R-PUBKEY-006** SHOULD: Establish baseline performance testing for authentication operations and document work factor selection rationale with periodic review cycles aligned to hardware capability trends.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite
# covering credential generation, hashing, and verification functions
echo "Running credential lifecycle test suite..."
# (Exact command derived from project's build tool and test configuration)

# Locate the project's static analysis or linting configuration and execute
# security-focused rules checking for plaintext credential storage
echo "Running static analysis for plaintext credential detection..."
# (Exact command derived from project's linting/analysis tooling)

# Identify the project's dependency scanning tooling and verify no known
# vulnerabilities exist in cryptographic or hashing dependencies
echo "Scanning cryptographic dependencies for vulnerabilities..."
# (Exact command derived from project's dependency scanning tool)
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries
- Work factor configuration is documented and baseline performance testing confirms authentication latency remains within acceptable bounds

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked. All credential lifecycle functions require explicit test coverage and security scanning validation before merge.
</enforcement>