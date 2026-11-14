---
version: 2
name: cross-cutting-teams-monitor-cpu-utilization-during-bdc8
description: "Adoption of bcryptjs for Credential Storage and Verification: Teams Monitor Cpu Utilization During Credential"
metadata:
  actual:
    adr_id: bdc8c843-7d3b-41ef-9803-0e67771a4da1
    status: accepted
    candidate_group_id: f6137ca5-465b-467c-86eb-54fd64264dd3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/utils/**
---

# Adoption of bcryptjs for Credential Storage and Verification: Teams Monitor Cpu Utilization During Credential

Scope: applies to `packages/db/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BCRYPT-001** SHOULD: Teams SHOULD monitor CPU utilization during credential authentication workflows and evaluate adjusting bcrypt salt rounds if hardware profiling indicates latency bottlenecks or security baseline changes.
- **R-BCRYPT-002** MANDATORY: Generate high-entropy tokens and persist verifiable proof of those secrets using cryptographic random bytes and one-way salted hashing.
- **R-BCRYPT-003** MANDATORY: Return the generated plaintext token to the client exactly once upon creation, as the unhashed secret cannot be retrieved from the persisted hash.
- **R-BCRYPT-004** MANDATORY: Execute credential comparison operations asynchronously using constant-time comparison methods to avoid blocking the event loop and prevent timing side-channel attacks.

### Verify

```bash
find . -type f -not -path '*/.*' -exec grep -l "bcrypt.hash" {} +
find . -type f -not -path '*/.*' -exec grep -l "bcrypt.compare" {} +
```

**Accept when:**
- All credential storage utilities invoke bcrypt.hash with ten salt rounds and avoid persisting raw token values.
- All token authentication verification paths use bcrypt.compare for credential validation.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests attempting to persist raw secrets or bypass bcrypt hashing will be blocked in code review.
</enforcement>