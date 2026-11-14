---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-ce1a
description: "Zod Runtime Schema Validation for Webhook Notification Boundaries: Engineers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: ce1aaf7c-957c-4832-8a1f-221c76c0fbbf
    status: accepted
    candidate_group_id: 00068acf-f0cb-4824-a15a-a745e550808c
    evidence:
      source: deep
  scope:
    root: packages/notifications/webhook
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Webhook Notification Boundaries: Engineers Inspect Repository Lock Artifact Determine

Scope: applies to `packages/notifications/webhook/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Engineers MUST inspect the repository lock artifact to determine the exact resolved version of Zod and verify that all invoked schema constructs exist within that resolved release prior to implementation.

### Verify

```bash
# Discover and execute the repository static analysis and type checking script to verify schema type adherence
npm run typecheck || pnpm typecheck || yarn typecheck

# Discover and run the project automated test suite to confirm schema validation passes on valid payloads and rejects malformed data
npm test || pnpm test || yarn test
```

**Accept when:**
- All automated tests covering notification ingestion and dispatch execute with passing results.
- Boundary schema parsing successfully rejects malformed payload structures and permits valid payload objects.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>