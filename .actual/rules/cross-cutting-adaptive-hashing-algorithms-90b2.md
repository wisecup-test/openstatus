# Adopt Asynchronous API Key Generation and Verification Pattern: Adaptive Hashing Algorithms

These rules are ALWAYS ACTIVE for all authentication modules that generate, hash, or verify API keys, including all credential storage operations, credential verification operations, and public API contracts exposed for API key lifecycle management.

### Rules

- **R-ASYNC-001** SHOULD: Adaptive hashing algorithms should use work factors that balance security requirements against acceptable latency for authentication operations.

### Verify

```bash
# Discover the project's cryptographic library and verify it provides asynchronous APIs for random byte generation
# Consult the library's documentation for the exact resolved version to determine correct method signatures and return types

# Discover the project's adaptive hashing library and verify it provides asynchronous APIs for both hash generation and comparison
# Configure work factors according to the security policy's requirements for authentication latency and computational cost

# Discover the project's test execution mechanism and run the authentication module's test suite
# Verify all credential operations return promises and complete without blocking

# Discover the project's static analysis configuration and execute linting rules
# Enforce proper async/await usage and promise handling in credential operations

# Discover the project's performance testing framework and execute load tests
# Verify authentication latency remains acceptable under concurrent request patterns matching production traffic profiles
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as asynchronous operations that return promises
- Static analysis confirms all credential operations use proper async/await patterns with comprehensive error handling
- Load tests demonstrate authentication latency remains within acceptable bounds under concurrent request patterns matching production traffic profiles
- All promises have rejection handlers and sensitive credential data is not exposed in error messages or stack traces
- Constant-time comparison is applied for credential verification to prevent timing attacks

<enforcement>
Claude Code MUST NOT skip or defer verification. All credential operations MUST be asynchronous. Synchronous cryptographic operations in authentication modules are violations. Code review MUST verify proper async/await usage and promise handling before merge approval.
</enforcement>