---
version: 2
name: cross-cutting-credential-hashes-computed-asynchronousl-9313
description: "Adoption of bcryptjs for Credential Storage and Verification: Credential Hashes Computed Asynchronously Bcrypt Hash"
metadata:
  actual:
    adr_id: 931398e4-3357-4f7b-ad67-ae4a9c67ee91
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: Credential Hashes Computed Asynchronously Bcrypt Hash

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BCRYPT-001** MUST: Credential hashes MUST be computed asynchronously using bcrypt.hash with a work factor of exactly 10 salt rounds.

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