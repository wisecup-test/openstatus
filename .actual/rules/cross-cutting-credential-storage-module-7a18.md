# Store API Key Credentials Using Salted Hash Functions: Credential Storage Module

These rules are ALWAYS ACTIVE for all credential storage operations, token generation for API keys, authentication verification flows, and database persistence of hashed credentials.

### Rules

- **R-CRED-001** MUST: The credential storage module MUST resolve the exact locked version of all cryptographic dependencies from the project's lock file before implementation.
- **R-CRED-002** MUST: API keys MUST be stored using salted hash functions with configurable work factor, never in plaintext or reversible encryption.
- **R-CRED-003** MUST: Token verification MUST use constant-time comparison to prevent timing-based attacks that could leak information about stored hashes.
- **R-CRED-004** MUST: Generated tokens MUST be presented to the user exactly once during creation and only the hashed value MUST be persisted to the database schema.
- **R-CRED-005** MUST: Cryptographic dependencies MUST be pinned to exact versions in the lock file; build tool output only verifies the active environment matches the lock file.
- **R-CRED-006** SHOULD: Usage tracking logic SHOULD compare current timestamp against last-used timestamp to determine whether an update is necessary, avoiding unnecessary write operations on every authentication.
- **R-CRED-007** SHOULD: Work factor tuning SHOULD be benchmarked against current hardware capabilities and monitored against cryptographic best practices.
- **R-CRED-008** SHOULD: Rate limiting SHOULD be implemented on authentication endpoints to mitigate denial-of-service attacks through repeated authentication attempts.

### Verify

```bash
# 1. Discover the project's test suite location and execute authentication module tests
find . -type f -name '*test*' -o -name '*spec*' | grep -i auth | head -5
# Execute: [build-tool] test --filter=auth

# 2. Locate the project's static analysis or linting configuration
find . -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name 'clippy.toml' \)
# Execute: [build-tool] lint --security

# 3. Identify the project's dependency audit tooling
find . -type f \( -name 'package-lock.json' -o -name 'Pipfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' \)
# Execute: [build-tool] audit

# 4. Verify credential storage module exports
grep -r "export.*hash\|export.*compare\|export.*random" . --include="*.js" --include="*.py" --include="*.go" --include="*.rs" | grep -i cred

# 5. Verify no plaintext credentials in database schema
grep -r "password\|token\|key" . --include="*.sql" --include="*schema*" | grep -v hash | grep -v encrypted
```

**Accept when:**
- All authentication module tests pass, including hash generation, verification with valid tokens, rejection of invalid tokens, and constant-time comparison behavior.
- Static analysis reports no plaintext credential storage violations and no use of deprecated or insecure cryptographic functions.
- Dependency audit shows no known vulnerabilities in cryptographic libraries at the resolved versions, or documented exceptions exist for accepted risks.
- Credential storage module exports hash, compare, and random byte generation functions imported from resolved cryptographic dependencies.
- Code review confirms API key tokens are presented to user exactly once during creation and only hashed values are persisted to database.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are non-negotiable. Verification commands MUST be executed before accepting any implementation. Static analysis and dependency audits MUST pass in CI pipeline before merge. Code review MUST verify hashing requirements and plaintext token handling before approval.
</enforcement>