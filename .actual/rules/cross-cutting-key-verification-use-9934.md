# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Key Verification Use

These rules are ALWAYS ACTIVE for all API key generation, hashing, and verification operations within the authentication subsystem.

### Rules

- **R-BCRYPT-001** MUST: API key verification MUST use constant-time comparison through bcrypt.compare to prevent timing attacks.

### Verify

```bash
# Discover the project's test execution configuration and run the authentication module's test suite
# to verify API key generation produces unique tokens
grep -r "test" . --include="package.json" --include="*.config.*" | head -5

# Discover the project's static analysis or linting configuration and verify that credential hashing
# operations use the required work factor
grep -r "bcrypt\|hash" . --include="*.js" --include="*.ts" | grep -E "(workFactor|rounds|cost)" | head -10

# Discover the project's dependency verification tooling and confirm the cryptographic libraries
# are pinned to exact versions in the lock artifact
grep -E "bcryptjs|crypto" package-lock.json yarn.lock pnpm-lock.yaml 2>/dev/null | head -5
```

**Accept when:**
- All API key generation tests pass, demonstrating cryptographically random token creation with sufficient entropy
- All API key verification tests pass, including constant-time comparison validation and hash mismatch handling
- Static analysis confirms no plaintext API key storage and all credential operations use the approved hashing interface
- Dependency verification confirms cryptographic libraries are locked to specific versions and match security baseline requirements

<enforcement>
Claude Code MUST NOT skip or defer verification. All API key verification operations MUST use bcrypt.compare for constant-time comparison. Violations block merge and trigger security team notification.
</enforcement>