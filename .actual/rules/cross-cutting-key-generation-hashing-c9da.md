# Use Asynchronous API Key Generation and Hashing for Credential Management: Key Generation Hashing

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-CRED-001** SHOULD: API key generation, hashing, and verification functions SHOULD be exposed through well-defined public contracts for consistent usage across the system.
- **R-CRED-002** MUST: All API key generation, hashing, and verification functions MUST be implemented as async functions to prevent blocking the event loop during cryptographic computations.
- **R-CRED-003** MUST: All asynchronous cryptographic operations MUST be properly awaited and wrapped in try-catch blocks to handle potential errors from underlying cryptographic libraries.
- **R-CRED-004** MUST: Cryptographically secure random byte generation MUST be used for API key generation (e.g., crypto.randomBytes).
- **R-CRED-005** MUST: Credential hashing MUST use bcrypt with a cost factor of 10 for password hashing, balancing security and performance.
- **R-CRED-006** MUST: No synchronous cryptographic operations are permitted in production code paths for API key generation, hashing, or verification.
- **R-CRED-007** SHOULD: Rate limiting SHOULD be implemented on authentication endpoints to prevent CPU saturation from concurrent high-volume authentication requests.
- **R-CRED-008** SHOULD: Periodic security reviews SHOULD be conducted to assess and adjust the bcrypt cost factor based on current hardware benchmarks.

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
grep -r "bcryptSync\|randomBytesSync" src/ --include="*.ts" --include="*.js" || echo "No synchronous crypto operations found"

# Verify async/await patterns are used at all cryptographic call sites
grep -r "await.*bcrypt\|await.*randomBytes" src/ --include="*.ts" --include="*.js"
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop
- No synchronous cryptographic operations (bcryptSync, randomBytesSync) are detected in production code
- All cryptographic function calls are wrapped in try-catch blocks or equivalent error handling
- Bcrypt cost factor is confirmed to be 10 in credential hashing implementation

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the scope of API key generation, hashing, verification, and credential storage operations.
</enforcement>