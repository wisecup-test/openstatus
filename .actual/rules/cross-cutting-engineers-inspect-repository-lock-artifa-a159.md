---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-a159
description: "Zod Schema Definition and Runtime Validation Library Adoption: Engineers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: a159d654-6598-4afb-98fd-57aeff2119a6
    status: accepted
    candidate_group_id: c1f28a38-ae70-4493-874a-5738669cad24
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation Library Adoption: Engineers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Engineers MUST inspect the repository lock artifact to determine the exact resolved version of Zod before introducing or modifying schema declarations and validation workflows.

### Verify

```bash
# Discover and execute the project test runner script defined in the repository dependency manifest to verify schema validation test suites pass.
# Discover and execute the static type verification script declared in the repository workspace configuration to ensure all schema-inferred types compile without errors.
# Discover and execute the project linter and format validation scripts to verify adherence to validation standards across all route and adapter files.
```

**Accept when:**
- All schema validation test suites pass successfully across boundary adapters and route handlers.
- Static type analysis passes with zero diagnostic errors across all modules defining or consuming schema-inferred types.
- Runtime input parsing reliably rejects malformed boundary payloads and permits valid payloads across all integration entry points.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>