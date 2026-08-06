# Use Asynchronous API Key Generation and Hashing for Credential Management: System Implement Last

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-CRED-001** MUST: Implement all API key generation operations as asynchronous functions to prevent blocking the event loop during cryptographic computations.
- **R-CRED-002** MUST: Implement all credential hashing and verification operations as asynchronous functions using bcrypt with a cost factor of 10.
- **R-CRED-003** MUST: Use `crypto.randomBytes` for cryptographically secure random byte generation in all API key generation operations.
- **R-CRED-004** MUST: Wrap all asynchronous cryptographic function calls in try-catch blocks to handle potential errors from underlying cryptographic libraries.
- **R-CRED-005** MUST: Properly await all cryptographic functions at call sites to ensure non-blocking execution.
- **R-CRED-006** MUST: Centralize all API key operations through public contracts including `generateApiKey`, `hashApiKey`, `verifyApiKeyHash`, and `shouldUpdateLastUsed`.
- **R-CRED-007** SHOULD: Implement comprehensive error handling with logging and monitoring for all async credential operations to prevent silent authentication failures.
- **R-CRED-008** SHOULD: Monitor authentication endpoint latency to detect performance degradation that might indicate the need for worker pool offloading.
- **R-CRED-009** MAY: Implement last-used timestamp logic to track API key usage patterns.
- **R-CRED-010** MUST NOT: Use synchronous cryptographic operations in production code paths.
- **R-CRED-011** MUST NOT: Use faster hashing algorithms like SHA-256 for credential storage; bcrypt's computational cost is a security feature.
- **R-CRED-EXC-001** Exception permitted: Synchronous hashing is allowed in test environments where blocking behavior is acceptable and performance is not critical.

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite
# covering API key generation, hashing, and verification operations
# (Exact command depends on project's build tool — inspect package.json or equivalent)

# Discover the project's static analysis configuration and execute type checking
# to verify all cryptographic functions return promises
# (Exact command depends on project's type checker — inspect tsconfig.json or equivalent)

# Discover the project's linting configuration and verify no synchronous
# cryptographic operations are used in production code paths
# (Exact command depends on project's linter — inspect .eslintrc or equivalent)

# Search for synchronous bcrypt or crypto operations in production code
grep -r "bcryptSync\|randomBytesSync" --include="*.ts" --include="*.js" src/ || echo "No synchronous crypto operations found"

# Verify async/await patterns are used for all credential operations
grep -r "async.*generateApiKey\|async.*hashApiKey\|async.*verifyApiKeyHash" --include="*.ts" --include="*.js" src/ || echo "Verify async patterns manually"
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop
- All asynchronous cryptographic operations are wrapped in try-catch blocks
- Comprehensive error handling with logging is implemented for credential operations
- Authentication endpoint latency remains within acceptable baseline thresholds under concurrent load

<enforcement>
Claude Code MUST NOT skip or defer verification. All cryptographic operations MUST be asynchronous in production code. Violations detected by static analysis MUST block CI pipeline and code review.
</enforcement>