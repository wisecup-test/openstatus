---
version: 2
name: cross-cutting-application-use-trpc-client-primary-94f7
description: "Adoption of @trpc/client for Type-Safe API Client Communication: Application Use Trpc Client Primary Library"
metadata:
  actual:
    adr_id: 94f7b8c2-aee4-4b40-940d-e8b7253da61f
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

# Adoption of @trpc/client for Type-Safe API Client Communication: Application Use Trpc Client Primary Library

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/forms/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: The application MUST use @trpc/client as the primary client library for executing type-safe queries, mutations, and remote procedure calls between the presentation layer and the backend service definition.
- **R-TRPC-002** MANDATORY: Discover the workspace dependency manifest and execute the project verification script to ensure all client and server module types align.
- **R-TRPC-003** MANDATORY: Discover the repository build script and run it to verify that client bundles do not contain prohibited server-only boundary imports.
- **R-TRPC-004** MANDATORY: Execute Lock-Version Grounding before writing code that uses a versioned library (inspect workspace manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm APIs exist, and re-run for version-sensitive behavior at point of use).

### Verify

```bash
# Discover the workspace dependency manifest and execute type checking / verification script
# Discover the repository build script and run it to check client bundles
# Discover and run the project test script
```

**Accept when:**
- Type verification completes with zero errors across all client queries, mutations, and server action procedure callers.
- Application build completes successfully without bundle boundary violations or missing module exports.
- All automated tests covering data fetching and mutation workflows pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking, build verification, and automated dependency boundary audits must pass.
</enforcement>