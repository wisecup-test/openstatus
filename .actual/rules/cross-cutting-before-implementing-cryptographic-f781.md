# Use Asynchronous API Key Generation and Hashing for Credential Management: Before Implementing Cryptographic

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-CRYPT-001** MUST: Before implementing cryptographic operations with any versioned library, discover the project's dependency lock artifact and resolve the exact installed version.
- **R-CRYPT-002** MUST: Implement all API key generation operations as asynchronous functions to prevent blocking the event loop.
- **R-CRYPT-003** MUST: Implement all credential hashing and verification operations as asynchronous functions.
- **R-CRYPT-004** MUST: Ensure all cryptographic functions are properly awaited and wrapped in try-catch blocks to handle potential errors.
- **R-CRYPT-005** MUST: Use cryptographically secure random byte generation (crypto.randomBytes) for API key generation.
- **R-CRYPT-006** MUST: Use bcrypt with a cost factor of 10 for password hashing and credential storage.
- **R-CRYPT-007** SHOULD: Implement comprehensive error handling with logging and monitoring for all async credential operations.
- **R-CRYPT-008** SHOULD: Monitor authentication endpoint latency to detect performance degradation.
- **R-CRYPT-009** MAY: Permit synchronous hashing in test environments where blocking behavior is acceptable and performance is not critical (EXC-001).

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite
# covering API key generation, hashing, and verification operations

# Discover the project's static analysis configuration and execute type checking
# to verify all cryptographic functions return promises

# Discover the project's linting configuration and verify no synchronous
# cryptographic operations are used in production code paths
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop
- Dependency lock artifact has been inspected to confirm exact versions of cryptographic libraries
- All cryptographic function calls are wrapped in try-catch blocks with error handling

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations in production code paths MUST trigger CI pipeline failure and code review blocking. Performance regression alerts MUST trigger investigation if authentication latency exceeds baseline thresholds.
</enforcement>