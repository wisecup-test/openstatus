---
version: 2
name: cross-cutting-audit-log-event-schemas-use-2b30
description: "Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Audit Log Event Schemas Use Discriminated"
metadata:
  actual:
    adr_id: 2b30c94f-f5cc-42f5-9455-c7096441a48b
    status: accepted
    candidate_group_id: e31ac072-3db1-4961-a5ee-464d55ca9a14
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Audit Log Event Schemas Use Discriminated

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all external data ingestion interfaces, telemetry schemas, audit log event contracts, and analytics client configuration parsing.

### Rules

- **R-ZOD-001** MUST: Audit log event schemas MUST use discriminated literal properties to isolate and constrain action-specific metadata structures.

### Verify

```bash
# Discover the project repository test runner and execute the automated test suites governing boundary schema validation and parsing contracts.
# Discover the project repository static analysis verification tools and execute type checking to ensure boundary interfaces infer types directly from validation schemas.
```

**Accept when:**
- All external ingestion payloads and client configuration objects pass validation against their respective Zod schemas with zero unhandled schema parse exceptions.
- Audit log event schemas enforce discriminated string literal action types with matching structured metadata properties across test suites.
- Dynamic timestamp attributes evaluate at invocation time rather than schema module initialization time.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external boundary interfaces must enforce Zod validation schemas.
</enforcement>