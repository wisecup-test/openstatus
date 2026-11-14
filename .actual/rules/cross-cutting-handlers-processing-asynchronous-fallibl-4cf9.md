---
version: 2
name: cross-cutting-handlers-processing-asynchronous-fallibl-4cf9
description: "Zod Schema Definition and Runtime Validation Library Adoption: Handlers Processing Asynchronous Fallible External Integration"
metadata:
  actual:
    adr_id: 4cf9d452-2145-43d0-8cf7-f4351872f5ad
    status: accepted
    candidate_group_id: c1f28a38-ae70-4493-874a-5738669cad24
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation Library Adoption: Handlers Processing Asynchronous Fallible External Integration

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all handlers processing asynchronous or fallible external integration payloads.

### Rules

- **R-ZOD-001** SHOULD: Handlers processing asynchronous or fallible external integration payloads SHOULD utilize safeParse to manage validation errors without throwing unhandled runtime exceptions.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration test and static type verification suites enforce these rules.
</enforcement>