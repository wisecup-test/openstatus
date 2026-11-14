---
version: 2
name: cross-cutting-consumers-inspect-authoritative-reposito-765b
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Consumers Inspect Authoritative Repository Lock Artifact"
metadata:
  actual:
    adr_id: 765b4bf8-b448-454e-a16b-f4c6a9e2faae
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Consumers Inspect Authoritative Repository Lock Artifact

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RPC-001** MUST: Consumers MUST inspect the authoritative repository lock artifact to determine the exact resolved dependency version before implementing or updating service handler routines.

### Verify

```bash
# Discover and run the project type-checking script using the designated repository runner
# Discover and execute the test runner target for handler unit and integration suites
# Execute the repository linting suite
```

**Accept when:**
- The repository type checker completes with zero errors across all RPC routing and handler modules.
- All test suites for RPC route registration and message conversion pass successfully.
- No untyped or ad-hoc request body parsers bypass the RPC framework boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>