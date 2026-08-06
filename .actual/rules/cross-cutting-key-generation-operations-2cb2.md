# Use Asynchronous API Key Generation and Hashing for Credential Management: Key Generation Operations

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-KEYGEN-001** MUST: All API key generation operations MUST use cryptographically secure random byte generation with a minimum entropy of 128 bits.
- **R-KEYGEN-002** MUST: All API key generation, hashing, and verification functions MUST be implemented as async functions and properly awaited at call sites.
- **R-KEYGEN-003** MUST: All asynchronous cryptographic operations MUST be wrapped in try-catch blocks to handle potential errors from underlying cryptographic libraries.
- **R-KEYGEN-004** MUST: No synchronous cryptographic operations are permitted in production code paths for credential management.
- **R-KEYGEN-005** MUST: Credential hashing MUST use bcrypt with a cost factor of 10 for password hashing, balancing security and performance.
- **R-KEYGEN-006** SHOULD: Implement comprehensive error handling with logging and monitoring for all async credential operations to prevent silent authentication failures.
- **R-KEYGEN-007** SHOULD: Implement rate limiting on authentication endpoints and monitor CPU utilization metrics to prevent saturation under high-volume concurrent requests.
- **R-KEYGEN-008** SHOULD: Establish periodic security reviews to assess and adjust the bcrypt cost factor based on current hardware benchmarks.

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite
# covering API key generation, hashing, and verification operations
# (Implementation: identify test runner from package.json or build config)

# Discover the project's static analysis configuration and execute type checking
# to verify all cryptographic functions return promises
# (Implementation: identify type checker from tsconfig.json or similar)

# Discover the project's linting configuration and verify no synchronous
# cryptographic operations are used in production code paths
# (Implementation: identify linter from .eslintrc or similar)

# Search for synchronous crypto operations in production code
grep -r "crypto\.randomBytes\s*\(" --include="*.ts" --include="*.js" | grep -v "async" | grep -v "await"
grep -r "bcrypt\.hash\s*\(" --include="*.ts" --include="*.js" | grep -v "async" | grep -v "await"
grep -r "bcrypt\.compare\s*\(" --include="*.ts" --include="*.js" | grep -v "async" | grep -v "await"
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop
- All cryptographic operations are wrapped in try-catch blocks with appropriate error handling
- No synchronous crypto.randomBytes, bcrypt.hash, or bcrypt.compare calls exist in production code

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for API key generation and credential management operations. Violations detected by static analysis in the CI pipeline MUST block merge requests.
</enforcement>