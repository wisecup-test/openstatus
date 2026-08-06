# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Key Generation Functions

These rules are ALWAYS ACTIVE for all public API authentication code paths, service-to-service authentication mechanisms, and client credential generation and management workflows.

### Rules

- **R-KEYGEN-001** MUST: API key generation functions must use cryptographically secure random number generation with sufficient entropy to prevent prediction attacks.
- **R-KEYGEN-002** MUST: Generate API keys with minimum 128-bit entropy from cryptographic random sources; encode in URL-safe format for transmission and storage in HTTP headers.
- **R-KEYGEN-003** MUST: Store API keys using adaptive hashing with configurable work factors rather than plaintext or reversible encryption.
- **R-KEYGEN-004** MUST: Configure adaptive hashing work factors based on authentication latency requirements; target 100-250ms per hash operation as baseline for acceptable user experience.
- **R-KEYGEN-005** SHOULD: Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication; consider caching last-used timestamps with eventual consistency.
- **R-KEYGEN-006** MUST: Enforce transport-layer security for all API endpoints and implement detection for plaintext credential transmission attempts in monitoring systems.

### Verify

```bash
# Discover the project's test execution configuration and run the test suite
# covering credential generation, hashing, and verification functions
find . -name 'package.json' -o -name 'go.mod' -o -name 'Gemfile' -o -name 'pom.xml' -o -name 'build.gradle' | head -1
# Then execute the project's test suite with security-critical path coverage

# Locate the project's static analysis or linting configuration and execute
# security-focused rules checking for plaintext credential storage
find . -name '.eslintrc*' -o -name 'golangci.yml' -o -name '.rubocop.yml' -o -name 'checkstyle.xml' | head -1
# Then run static analysis with security rules enabled

# Identify the project's dependency scanning tooling and verify no known
# vulnerabilities exist in cryptographic or hashing dependencies
find . -name 'package-lock.json' -o -name 'go.sum' -o -name 'Gemfile.lock' -o -name 'pom.lock' -o -name 'gradle.lock' | head -1
# Then execute dependency vulnerability scanning
```

**Accept when:**
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries
- Authentication latency measurements confirm hashing operations complete within 100-250ms baseline
- Code review verification confirms explicit use of cryptographic library APIs documented for the exact resolved dependency version

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing plaintext credential storage or weak random generation are automatically blocked. All cryptographic library usage must be verified against exact resolved versions before implementation.
</enforcement>