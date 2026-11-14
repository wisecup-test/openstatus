---
version: 2
name: cross-cutting-schema-parsing-executed-ingress-boundary-65c2
description: "Zod Runtime Schema Validation for Notification Payloads: Schema Parsing Executed Ingress Boundary Zod"
metadata:
  actual:
    adr_id: 65c2f383-fa2f-4543-84ec-a064cda5e3bc
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Schema Parsing Executed Ingress Boundary Zod

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Schema parsing MUST be executed at the ingress boundary using the zod parse method on deserialized data prior to invoking external client dispatch operations.

### Verify

```bash
# Discover and run the project test runner script from the repository manifest
# Discover and run the repository linter script from the root configuration
```

**Accept when:**
- All notification payload validation tests pass successfully with valid and invalid input fixtures.
- Static analysis and type checking pass without violations across notification modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification payload processing must strictly execute Zod parsing at the ingress boundary.
</enforcement>