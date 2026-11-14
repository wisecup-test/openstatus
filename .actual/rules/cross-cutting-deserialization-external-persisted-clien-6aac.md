---
version: 2
name: cross-cutting-deserialization-external-persisted-clien-6aac
description: "Zod Schema Validation for Form and Component Input Boundaries: Deserialization External Persisted Client Payloads Utilize"
metadata:
  actual:
    adr_id: 6aac4c99-8b19-46f7-adbf-de2f4fec8d8b
    status: accepted
    candidate_group_id: df6eea8a-3ab7-4843-bbef-10688bb78119
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
      - src/lib/**
      - src/scripts/**
---

# Zod Schema Validation for Form and Component Input Boundaries: Deserialization External Persisted Client Payloads Utilize

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/**`, `apps/dashboard/src/scripts/**`.

These rules are ALWAYS ACTIVE for all user-facing dashboard forms, input dialogs, configuration panels, and deserialization/hydration boundaries for client-persisted session data and search parameter caches.

### Rules

- **R-ZOD-001** MUST: Deserialization of external or persisted client payloads MUST utilize safeParse and explicitly handle validation failure branches before consuming the data.

### Verify

```bash
# Discover and execute static analysis scripts and test runner suites
# (e.g., run project test and linter commands derived from the repository manifest)
```

**Accept when:**
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>