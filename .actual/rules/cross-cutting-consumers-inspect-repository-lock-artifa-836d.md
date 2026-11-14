---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-836d
description: "Adopt Hono Framework for Workflow Service Routing and Middleware: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 836d4713-be12-4e04-ab6f-2f5d752d9b6d
    status: accepted
    candidate_group_id: 21dd5eee-ded5-43d1-9312-6f96f5101826
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
      - src/incident/**
---

# Adopt Hono Framework for Workflow Service Routing and Middleware: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of the adopted routing framework before implementing endpoint handlers, verifying that all invoked API surfaces exist in that version's authoritative documentation.

### Verify

```bash
# Discover the project dependency manifest, build tool, and lock artifact to verify framework versions
# Discover and execute project test suite covering workflow routing endpoints
# Run project static analysis, linting, and type validation scripts against all route definition modules
```

**Accept when:**
- All workflow endpoints are registered on framework router instances and successfully pass automated HTTP integration tests.
- Requests omitting valid authorization headers are rejected with an unauthorized status prior to downstream handler execution.
- Type validation and lint verification scripts execute without errors across all router and handler modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>