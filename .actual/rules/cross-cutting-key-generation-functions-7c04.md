# Adopt Asynchronous API Key Generation and Verification Pattern: Key Generation Functions

These rules are ALWAYS ACTIVE for all authentication modules that generate, hash, or verify API keys, including all credential storage operations and public API contracts exposed for API key lifecycle management.

### Rules

- **R-ASYNC-KEY-001** MUST: All API key generation functions that produce cryptographically secure random tokens shall be implemented as asynchronous operations returning promises.
- **R-ASYNC-KEY-002** MUST: All credential storage operations that hash tokens before persistence shall be implemented as asynchronous operations.
- **R-ASYNC-KEY-003** MUST: All credential verification operations that compare provided tokens against stored hashes shall be implemented as asynchronous operations.
- **R-ASYNC-KEY-004** MUST: All public API contracts exposed for API key lifecycle management shall use consistent asynchronous interfaces.
- **R-ASYNC-KEY-005** MUST: Implement comprehensive error handling with try-catch blocks for all asynchronous credential operations without exposing sensitive credential data in error messages or stack traces.
- **R-ASYNC-KEY-006** MUST: Apply constant-time comparison for credential verification to prevent timing attacks.
- **R-ASYNC-KEY-007** MUST: Enforce strict linting rules for promise handling and async/await usage in authentication modules.

### Verify

```bash
# Discover the project's cryptographic library and verify asynchronous APIs
grep -r "crypto" package.json package-lock.json yarn.lock pnpm-lock.yaml 2>/dev/null | head -5

# Discover the project's adaptive hashing library
grep -r "bcrypt" package.json package-lock.json yarn.lock pnpm-lock.yaml 2>/dev/null | head -5

# Run the authentication module's test suite
npm test -- --testPathPattern=auth 2>/dev/null || yarn test --testPathPattern=auth 2>/dev/null || echo "Test command not found"

# Execute linting rules that enforce async/await usage
npm run lint 2>/dev/null || yarn lint 2>/dev/null || echo "Lint command not found"

# Verify all credential operations return promises
grep -r "async\|Promise" src/auth src/authentication 2>/dev/null | grep -E "generateApiKey|hashApiKey|verifyApiKeyHash|shouldUpdateLastUsed" | head -10

# Execute load tests if available
npm run test:load 2>/dev/null || yarn test:load 2>/dev/null || echo "Load test command not configured"
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as asynchronous operations that return promises
- Static analysis confirms all credential operations use proper async/await patterns with comprehensive error handling
- Load tests demonstrate authentication latency remains within acceptable bounds under concurrent request patterns
- No synchronous credential operations exist in the authentication module
- Error handling in credential operations does not expose sensitive data in logs or stack traces
- Constant-time comparison is applied to all credential verification operations

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication modules. Violations must be identified and corrected before code merge.
</enforcement>