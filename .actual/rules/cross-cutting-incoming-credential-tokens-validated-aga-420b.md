---
version: 2
name: cross-cutting-incoming-credential-tokens-validated-aga-420b
description: "Adoption of bcryptjs for Credential Storage and Verification: Incoming Credential Tokens Validated Against Stored"
metadata:
  actual:
    adr_id: 420ba64f-52d5-47b8-a2d5-0c089d194a14
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: Incoming Credential Tokens Validated Against Stored

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BCRYPT-001** MUST: Incoming credential tokens MUST be validated against stored hashes using bcrypt.compare to ensure constant-time comparison and prevent timing attack vulnerabilities.

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