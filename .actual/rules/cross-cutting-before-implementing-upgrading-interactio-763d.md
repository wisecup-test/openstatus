---
version: 2
name: cross-cutting-before-implementing-upgrading-interactio-763d
description: "Adoption of @openstatus/ui Status Block Components and Shared Types: Before Implementing Upgrading Interactions Package Consumer"
metadata:
  actual:
    adr_id: 763df835-d7d9-4f8e-99b7-e401af2b88c9
    status: accepted
    candidate_group_id: 6ffc73a3-906b-49a6-bf40-eca1e97f23d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/ui Status Block Components and Shared Types: Before Implementing Upgrading Interactions Package Consumer

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for status dashboard pages, incident timeline views, service history displays, and data aggregation functions that map telemetry and incident records into daily status intervals.

### Rules

- **R-OPN-001** MUST: Before implementing or upgrading interactions with the UI package, the consumer MUST inspect the workspace dependency lock artifact and resolve the exact locked version of the library.
- **R-OPN-002** MUST: Import status types and status bar components using canonical module resolution paths provided by the UI package.
- **R-OPN-003** MUST: Ensure client components isolate stateful interactivity while maintaining purity in underlying status aggregation functions.

### Verify

```bash
# Discover and execute the repository type-checking script across all workspace packages
# Discover and run the repository linter to detect unapproved local status type definitions
# Execute the automated test suite to verify correct rendering and aggregation of status timeline components
```

**Accept when:**
- All components rendering status timelines import presentation blocks and type contracts exclusively from @openstatus/ui.
- Static type checking and automated test suites confirm zero type mismatches across status calculations and UI bindings.
- Workspace verification pipelines pass with zero unresolved dependencies or unregistered module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>