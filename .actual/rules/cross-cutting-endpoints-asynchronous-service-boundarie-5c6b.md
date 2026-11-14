---
version: 2
name: cross-cutting-endpoints-asynchronous-service-boundarie-5c6b
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Endpoints Asynchronous Service Boundaries Validate Incoming"
metadata:
  actual:
    adr_id: 5c6bee8f-729c-4e4b-80f1-872f747e3728
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Endpoints Asynchronous Service Boundaries Validate Incoming

Scope: applies to `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all incoming request payloads, query parameters, external client responses, environment variables, and structured content parsing schemas across application ingress surfaces.

### Rules

- **R-ZOD-001** MUST: API endpoints and asynchronous service boundaries MUST validate incoming request payloads and query parameters using safeParse to isolate schema parsing failures without raising unhandled exceptions.

### Verify

```bash
# Execute repository type-checking pipeline discovered from workspace configuration
npm run type-check || pnpm type-check || yarn type-check

# Execute test runner discovered from project automation scripts
npm test || pnpm test || yarn test

# Execute linting and static analysis suite
npm run lint || pnpm lint || yarn lint
```

**Accept when:**
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer code reviews verify adherence; unvalidated boundary inputs or manual type casts at external boundaries are strictly blocked.
</enforcement>