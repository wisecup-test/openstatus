---
version: 2
name: cross-cutting-plaintext-credential-tokens-not-persiste-8721
description: "Adoption of bcryptjs for Credential Storage and Verification: Plaintext Credential Tokens Not Persisted Durable"
metadata:
  actual:
    adr_id: 87212200-4abc-41d3-a99f-e838accb0b57
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: Plaintext Credential Tokens Not Persisted Durable

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BCRYPT-001** MUST_NOT: Plaintext credential tokens MUST NOT be persisted to durable storage, written to log sinks, or retained in memory after the initial presentation and hashing cycle.
- **R-BCRYPT-002** MANDATORY: Always return the generated plaintext token to the client exactly once upon creation, as the unhashed secret cannot be retrieved from the persisted hash.
- **R-BCRYPT-003** MANDATORY: Execute credential comparison operations asynchronously to avoid blocking the event loop during password and token verification.

### Verify

```bash
find . -type f -not -path '*/.*' -exec grep -l "bcrypt.hash" {} +
find . -type f -not -path '*/.*' -exec grep -l "bcrypt.compare" {} +
```

**Accept when:**
- All credential storage utilities invoke bcrypt.hash with ten salt rounds and avoid persisting raw token values.
- All token authentication verification paths use bcrypt.compare for credential validation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, code review checks, and test suites validate that credential models do not store raw tokens and use bcrypt hashing.
</enforcement>