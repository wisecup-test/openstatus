# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: System Expose Separate

These rules are ALWAYS ACTIVE for all code implementing API key generation, hashing, verification, and usage tracking in authentication systems.

### Rules

- **R-BCRYPT-001** MUST: Generate API keys using cryptographic random generation from the system's cryptographic module, encoding the result in URL-safe format.
- **R-BCRYPT-002** MUST: Hash API keys using adaptive hashing (bcrypt) with a work factor of at least 10 before storing in persistent data stores.
- **R-BCRYPT-003** MUST: Implement API key verification using only the hashing library's built-in constant-time comparison function; do not implement custom comparison logic.
- **R-BCRYPT-004** MUST: Return both the plain token to the client and the hashed version for storage during key generation.
- **R-BCRYPT-005** SHOULD: Implement usage tracking with conditional updates using a time threshold to avoid unnecessary database writes on every authentication.
- **R-BCRYPT-006** SHOULD: Separate generation, hashing, and verification logic into distinct functions to enable independent unit testing and performance benchmarking.
- **R-BCRYPT-007** MAY: Expose separate public contracts for generation, hashing, verification, and usage tracking to support modular testing and deployment.
- **R-BCRYPT-008** MUST NOT: Use plain-text credential storage or non-cryptographic random generation for API keys.
- **R-BCRYPT-009** MUST NOT: Use HMAC-SHA256 or reversible encryption as the primary API key storage mechanism.

### Verify

```bash
# Discover and run the project's test suite covering API key generation, hashing, and verification
# Confirm all tests pass and verify cryptographic properties (entropy, work factor compliance)

# Discover and execute the project's static analysis or linting configuration
# Verify no instances of plain-text credential storage or weak random generation are detected

# Discover and run the project's dependency scanning configuration
# Confirm the adaptive hashing library is present at the locked version with no high or critical vulnerabilities
```

**Accept when:**
- All tests for API key generation produce tokens with sufficient entropy and all hashing tests verify work factor compliance
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation
- Dependency scanning confirms the hashing library is at the locked version with no high or critical severity vulnerabilities
- Code review confirms separation of generation, hashing, and verification logic into distinct functions
- Verification logic uses only the hashing library's built-in constant-time comparison function

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-BCRYPT-001 through R-BCRYPT-009 are mandatory for API key implementation. Violations in static analysis or test failures block merge. Security team review is required for any exceptions, which must include compensating controls and time-bound remediation.
</enforcement>