# Use Asynchronous API Key Generation and Hashing for Credential Management: Credential Hashing Operations

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-CRED-001** MUST: All credential hashing operations MUST be implemented as asynchronous functions to prevent blocking the event loop.
- **R-CRED-002** MUST: All API key generation operations MUST use cryptographically secure random byte generation.
- **R-CRED-003** MUST: All cryptographic functions MUST be properly awaited and wrapped in try-catch blocks to handle potential errors from underlying cryptographic libraries.
- **R-CRED-004** MUST: All credential hashing and verification operations MUST be exposed through centralized public contracts (generateApiKey, hashApiKey, verifyApiKeyHash, shouldUpdateLastUsed).
- **R-CRED-005** MUST: Bcrypt hashing MUST use a cost factor of 10 for password hashing, balancing security and performance.
- **R-CRED-006** SHOULD: Implement comprehensive error handling with logging and monitoring for all async credential operations to prevent silent authentication failures.
- **R-CRED-007** SHOULD: Monitor authentication endpoint latency to detect performance degradation that might indicate the need for worker pool offloading.
- **R-CRED-008** MAY: Synchronous hashing is permitted in test environments where blocking behavior is acceptable and performance is not critical (EXC-001).

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite covering API key generation, hashing, and verification operations
# (Exact command depends on project's build tool — inspect package.json or equivalent)

# Discover the project's static analysis configuration and execute type checking to verify all cryptographic functions return promises
# (Exact command depends on project's type checker — inspect tsconfig.json or equivalent)

# Discover the project's linting configuration and verify no synchronous cryptographic operations are used in production code paths
# (Exact command depends on project's linter — inspect .eslintrc or equivalent)
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop
- All cryptographic operations include proper error handling with try-catch blocks
- Bcrypt cost factor is verified to be set to 10 in all credential hashing operations

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis in the continuous integration pipeline MUST check for synchronous cryptographic operations. Code review MUST verify async/await patterns for all credential operations. Performance testing MUST measure authentication endpoint latency under concurrent load. CI pipeline MUST fail if synchronous cryptographic operations are detected in production code paths.
</enforcement>