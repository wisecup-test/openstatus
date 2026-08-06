# Use Asynchronous API Key Generation and Hashing for Credential Management: Key Verification Operations

These rules are ALWAYS ACTIVE for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

### Rules

- **R-CRED-001** MUST: All API key verification operations MUST be implemented as asynchronous functions using constant-time comparison algorithms.

### Verify

```bash
# Discover the project's test execution mechanism and run the test suite covering API key generation, hashing, and verification operations
# (Exact command depends on project's build tool — inspect package.json or equivalent manifest)

# Discover the project's static analysis configuration and execute type checking to verify all cryptographic functions return promises
# (Run the configured type checker to confirm async/await patterns)

# Discover the project's linting configuration and verify no synchronous cryptographic operations are used in production code paths
# (Run the configured linter with rules targeting blocking crypto operations)
```

**Accept when:**
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis in the continuous integration pipeline MUST check for synchronous cryptographic operations. Code review MUST verify async/await patterns for all credential operations. Performance testing MUST measure authentication endpoint latency under concurrent load. Violations block merge requests and trigger CI pipeline failures.
</enforcement>