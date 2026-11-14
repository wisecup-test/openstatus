---
version: 2
name: cross-cutting-new-credential-tokens-generated-cryptogr-a109
description: "Adoption of bcryptjs for Credential Storage and Verification: New Credential Tokens Generated Cryptographically Secure"
metadata:
  actual:
    adr_id: a10971f5-286d-42b1-8828-661e1ce8a1ea
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: New Credential Tokens Generated Cryptographically Secure

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AUTH-001** MUST: New credential tokens MUST be generated using cryptographically secure pseudorandom byte generation via crypto.randomBytes with a minimum entropy size of 16 bytes prior to hashing.

### Verify

```bash
find . -type f -not -path '*/.*' -exec grep -l "bcrypt.hash" {} +
find . -type f -not -path '*/.*' -exec grep -l "bcrypt.compare" {} +
```

**Accept when:**
- All credential storage utilities invoke bcrypt.hash with ten salt rounds and avoid persisting raw token values.
- All token authentication verification paths use bcrypt.compare for credential validation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>