---
version: 2
name: cross-cutting-key-tokens-authentication-credentials-st-e016
description: "Adoption of bcryptjs for Credential Storage and Verification: Key Tokens Authentication Credentials Stored One"
metadata:
  actual:
    adr_id: e016c2e9-0ab6-44a1-8227-9146d543197a
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: Key Tokens Authentication Credentials Stored One

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BCRYPT-001** MUST: All API key tokens and authentication credentials MUST be stored as one-way salted hashes using bcryptjs rather than plaintext or reversible encodings.

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