# Use Asynchronous API Key Generation and Hashing for Credential Management: Credential Storage Use

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-CRED-001** MUST: Credential storage MUST use bcrypt hashing with a minimum cost factor of 10.

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite covering API key generation, hashing, and verification operations
# Discover the project's static analysis configuration and execute type checking to verify all cryptographic functions return promises
# Discover the project's linting configuration and verify no synchronous cryptographic operations are used in production code paths
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop
- Bcrypt cost factor is verified to be at least 10 in all credential storage implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations detected by automated static analysis in the CI pipeline MUST block merge requests. Code review MUST verify async/await patterns for all credential operations before approval.
</enforcement>