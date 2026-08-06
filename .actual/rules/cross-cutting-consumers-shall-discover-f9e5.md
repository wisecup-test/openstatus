# Adopt Asynchronous API Key Generation and Verification Pattern: Consumers Shall Discover

These rules are ALWAYS ACTIVE for all authentication modules that generate, hash, or verify API keys, including all credential storage operations and public API contracts exposed for API key lifecycle management.

### Rules

- **R-ASYNC-001** MUST: Consumers shall discover the exact resolved version of all cryptographic and hashing libraries from the project's dependency lock artifact before implementing credential operations.
- **R-ASYNC-002** MUST: All API key generation, hashing, and verification functions shall be implemented as asynchronous operations that return promises.
- **R-ASYNC-003** MUST: Implement all credential operations using async/await syntax with comprehensive try-catch blocks.
- **R-ASYNC-004** MUST: Ensure promise rejections are properly handled and logged without exposing sensitive credential data in error messages or stack traces.
- **R-ASYNC-005** MUST: Apply constant-time comparison for credential verification to prevent timing attacks.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock artifact
# Identify the build tool and inspect the lock file for exact resolved versions of crypto and hashing libraries

# 2. Verify all credential operations return promises
# Run the authentication module's test suite
npm test -- --testPathPattern=auth

# 3. Execute static analysis to enforce async/await usage
# Discover the project's linting configuration and run it
npm run lint -- --rule async-await

# 4. Run load tests for concurrent authentication patterns
# Discover the project's performance testing framework
npm run test:performance -- --scenario concurrent-auth

# 5. Verify no synchronous credential operations exist
# Search for synchronous crypto calls in authentication modules
grep -r "crypto\.randomBytes\|bcrypt\.hash\|bcrypt\.compare" src/auth --include="*.js" | grep -v "await\|async" || echo "No synchronous crypto calls found"
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as asynchronous operations that return promises
- Static analysis confirms all credential operations use proper async/await patterns with comprehensive error handling
- Load tests demonstrate authentication latency remains within acceptable bounds under concurrent request patterns matching production traffic profiles
- No synchronous cryptographic operations are found in authentication modules
- Promise rejections are properly handled with no sensitive data exposed in error messages

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication modules. Violations block pull requests in the CI pipeline.
</enforcement>