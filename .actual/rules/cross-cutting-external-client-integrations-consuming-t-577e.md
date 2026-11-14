---
version: 2
name: cross-cutting-external-client-integrations-consuming-t-577e
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: External Client Integrations Consuming Third Party"
metadata:
  actual:
    adr_id: 577e5eab-6ce3-48a9-96d2-67d53e12df34
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: External Client Integrations Consuming Third Party

Scope: applies to `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: External client integrations consuming third-party HTTP responses MUST parse untrusted response bodies through defined Zod schemas prior to dispatching data to downstream domain services.

### Verify

\`\`\`bash
# Execute repository type-checking pipeline discovered from workspace configuration
npm run typecheck
# Execute test runner discovered from project automation scripts
npm test
# Execute linting and static analysis suite
npm run lint
\`\`\`

**Accept when:**
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>