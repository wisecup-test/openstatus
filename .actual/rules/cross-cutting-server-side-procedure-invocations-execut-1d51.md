---
version: 2
name: cross-cutting-server-side-procedure-invocations-execut-1d51
description: "Adoption of @trpc/client for Type-Safe API Client Communication: Server Side Procedure Invocations Executing Outside"
metadata:
  actual:
    adr_id: 1d51815c-0f5e-4b54-96fc-2ac66bf3b773
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

# Adoption of @trpc/client for Type-Safe API Client Communication: Server Side Procedure Invocations Executing Outside

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/forms/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client components, UI forms, page views, and server action handlers requiring remote procedure invocation or data synchronization with backend services.

### Rules

- **R-TRPC-001** MUST: Server-side procedure invocations executing outside client contexts MUST isolate server-specific dependencies using environment-boundary guards to prevent server-only logic from leaking into client bundles.

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