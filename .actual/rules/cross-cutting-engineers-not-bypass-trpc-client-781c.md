---
version: 2
name: cross-cutting-engineers-not-bypass-trpc-client-781c
description: "Adoption of @trpc/client for Type-Safe API Client Communication: Engineers Not Bypass Trpc Client Construct"
metadata:
  actual:
    adr_id: 781cc934-0fd6-40b1-8181-438589cdcc5c
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

# Adoption of @trpc/client for Type-Safe API Client Communication: Engineers Not Bypass Trpc Client Construct

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/forms/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client components, UI forms, page views, and server action handlers requiring remote procedure invocation or data synchronization with backend services.

### Rules

- **R-TRPC-001** MUST_NOT: Engineers MUST_NOT bypass @trpc/client to construct ad-hoc HTTP endpoints or untyped fetch calls for operations already exposed via typed procedures.

### Verify

```bash
# Discover workspace dependency manifest and execute project type-checking/verification script
# Discover repository build script and run it to verify client bundles do not contain prohibited server-only boundary imports
# Discover and run project test script to ensure all query and mutation hooks execute as expected
```

**Accept when:**
- Type verification completes with zero errors across all client queries, mutations, and server action procedure callers.
- Application build completes successfully without bundle boundary violations or missing module exports.
- All automated tests covering data fetching and mutation workflows pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>