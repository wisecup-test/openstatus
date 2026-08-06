# Adopt Asynchronous API Key Generation and Verification Pattern: Authentication Modules Implement

These rules are ALWAYS ACTIVE for all authentication modules that generate, hash, or verify API keys for programmatic access to services.

### Rules

- **R-AUTH-001** MUST: Implement all API key generation, hashing, and verification functions as asynchronous operations that return promises.
- **R-AUTH-002** MUST: Use async/await syntax with comprehensive try-catch blocks for all credential operations to prevent blocking the event loop.
- **R-AUTH-003** MUST: Ensure all promise rejections are properly handled and logged without exposing sensitive credential data in error messages or stack traces.
- **R-AUTH-004** MUST: Apply constant-time comparison for credential verification to prevent timing attacks.
- **R-AUTH-005** SHOULD: Implement credential refresh policies that determine when stored hashes should be updated based on last usage timestamps.
- **R-AUTH-006** SHOULD: Configure adaptive hashing work factors according to the security policy's requirements for authentication latency and computational cost.

### Verify

```bash
# 1. Discover the project's test execution mechanism and run the authentication module's test suite
# to verify all credential operations return promises and complete without blocking
echo "Running authentication module test suite..."
# (Exact command depends on project's test framework - discovered from package.json or build config)

# 2. Discover the project's static analysis configuration and execute linting rules
# that enforce proper async/await usage and promise handling in credential operations
echo "Running static analysis for async/await patterns..."
# (Exact command depends on project's linter - discovered from .eslintrc or similar)

# 3. Discover the project's performance testing framework and execute load tests
# that verify authentication latency remains acceptable under concurrent request patterns
echo "Running performance tests for concurrent authentication..."
# (Exact command depends on project's performance testing tool - discovered from test configuration)
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as asynchronous operations that return promises
- Static analysis confirms all credential operations use proper async/await patterns with comprehensive error handling
- Load tests demonstrate authentication latency remains within acceptable bounds under concurrent request patterns matching production traffic profiles
- No synchronous cryptographic operations are detected in credential lifecycle management code
- All promise rejections include proper error handling without leaking sensitive credential information

<enforcement>
Claude Code MUST NOT skip or defer verification. All credential operations MUST be asynchronous. Synchronous implementations are violations and MUST be rejected.
</enforcement>