---
version: 2
name: cross-cutting-before-implementing-modifying-credential-bbc1
description: "Adoption of bcryptjs for Credential Storage and Verification: Before Implementing Modifying Credential Hashing Routines"
metadata:
  actual:
    adr_id: bbc1952b-85c4-4adc-9ed9-861b98c79848
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: Before Implementing Modifying Credential Hashing Routines

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for generation, persistence, and verification of API key tokens, authentication credentials, and data layer utilities handling credential storage.

### Rules

- **R-BCRYPT-001** MUST: Before implementing or modifying credential hashing routines, consumers MUST discover the project dependency manifest and lock artifact to resolve and verify the exact installed version of the bcryptjs library.

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