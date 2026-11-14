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
- **R-BCRYPT-002** MANDATORY: Generate high-entropy tokens using the repository's cryptographic utilities (such as crypto.randomBytes with 16 bytes) and persist only the one-way salted bcrypt hash.
- **R-BCRYPT-003** MANDATORY: Always return the generated plaintext token to the client exactly once upon creation, as the unhashed secret cannot be retrieved from the persisted hash.
- **R-BCRYPT-004** MANDATORY: Execute credential comparison operations asynchronously using constant-time verification methods (such as bcrypt.compare) to avoid timing side-channel attacks and event loop blocking.
- **R-BCRYPT-005** MANDATORY: Execute discovery and lock-version grounding procedures (identifying dependency manifests, lock files, exact resolved versions, and official documentation) before implementing or modifying credential storage or verification code.

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