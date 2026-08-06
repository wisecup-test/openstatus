# Adopt Asynchronous API Key Generation and Verification Pattern: Key Hashing Functions

These rules are ALWAYS ACTIVE for all authentication modules that generate, hash, or verify API keys.

### Rules

- **R-ASYNC-001** MUST: All API key hashing functions that apply adaptive hashing algorithms shall be implemented as asynchronous operations to prevent event loop blocking.

### Verify

```bash
# Discover the project's cryptographic library and verify it provides asynchronous APIs for random byte generation
# Consult the library's documentation for the exact resolved version to determine correct method signatures

# Discover the project's adaptive hashing library and verify it provides asynchronous APIs for both hash generation and comparison
# Configure work factors according to security policy requirements

# Run the authentication module's test suite to verify all credential operations return promises and complete without blocking
npm test -- --testPathPattern=auth

# Execute linting rules that enforce proper async/await usage and promise handling in credential operations
npm run lint -- --rule async-await

# Execute load tests that verify authentication latency remains acceptable under concurrent request patterns
npm run test:load -- --concurrency=100
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as asynchronous operations that return promises
- Static analysis confirms all credential operations use proper async/await patterns with comprehensive error handling
- Load tests demonstrate authentication latency remains within acceptable bounds under concurrent request patterns matching production traffic profiles
- All credential operations use async/await syntax with comprehensive try-catch blocks
- Promise rejections are properly handled and logged without exposing sensitive credential data

<enforcement>
Claude Code MUST NOT skip or defer verification. All credential operations must be confirmed as asynchronous before acceptance.
</enforcement>