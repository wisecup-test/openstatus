---
version: 2
name: cross-cutting-remote-procedure-call-configurations-sha-7722
description: "Adoption of @trpc/client for Type-Safe API Client Communication: Remote Procedure Call Configurations Share Centralized"
metadata:
  actual:
    adr_id: 7722cac5-9b76-4e05-9151-e257f9b6acdd
    status: accepted
    candidate_group_id: 14b97067-d036-4c05-b4e7-cb525f0cce77
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/forms/**
      - src/lib/trpc/**
---

# Adoption of @trpc/client for Type-Safe API Client Communication: Remote Procedure Call Configurations Share Centralized

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/forms/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client components, UI forms, page views, and server action handlers requiring remote procedure invocation or data synchronization with backend services.

### Rules

- **R-RPC-001** SHOULD: All remote procedure call configurations share centralized serialization transformers and error-handling instrumentation between client and server communication instances.

### Verify

```bash
# Discover the workspace dependency manifest and execute the project verification script to ensure all client and server module types align.
# Discover the repository build script and run it to verify that client bundles do not contain prohibited server-only boundary imports.
# Discover and run the project test script to ensure all query and mutation hooks execute as expected.
```

**Accept when:**
- Type verification completes with zero errors across all client queries, mutations, and server action procedure callers.
- Application build completes successfully without bundle boundary violations or missing module exports.
- All automated tests covering data fetching and mutation workflows pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>