---
version: 2
name: cross-cutting-external-client-modules-not-return-4ef1
description: "Zod Schema Validation for External API Client Boundaries: External Client Modules Not Return Unparsed"
metadata:
  actual:
    adr_id: 4ef1e99b-1464-4096-a882-50fa3fa39ffc
    status: accepted
    candidate_group_id: 2a48e15c-71e7-4ab2-94a0-84daf33ebd71
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External API Client Boundaries: External Client Modules Not Return Unparsed

Scope: applies to `packages/importers/src/providers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: External client modules MUST NOT return unparsed or unsanitized external response bodies across the boundary interface.

### Verify

```bash
# Discover and run the repository test runner targeting external integration client suites to verify payload parsing and error handling
# Discover and execute the repository static analysis linter to confirm that external client fetch responses pass through schema validation
```

**Accept when:**
- All external client integration tests pass, confirming schema validation executes on network response payloads.
- No unvalidated external response payloads are exposed directly through provider client interfaces.
- Schema parsing rejections yield structured validation errors rather than unhandled raw network exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>