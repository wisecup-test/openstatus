---
version: 2
name: cross-cutting-external-telemetry-payloads-audit-log-7201
description: "Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: External Telemetry Payloads Audit Log Events"
metadata:
  actual:
    adr_id: 72019a20-2913-4e85-a9e7-5633ad5d3bed
    status: accepted
    candidate_group_id: e31ac072-3db1-4961-a5ee-464d55ca9a14
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: External Telemetry Payloads Audit Log Events

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all external data ingestion interfaces, telemetry schemas, audit log event contracts, and analytics client configuration parsing.

### Rules

- **R-ZOD-001** MUST: All external telemetry payloads, audit log events, and client configuration inputs crossing external boundaries MUST be validated using Zod runtime schemas before consumption or datastore ingestion.
- **R-ZOD-002** MUST: Compose granular reusable base schemas into higher-level action schemas using object extension and literal discriminators.
- **R-ZOD-003** MUST: Utilize safe parsing wrappers when handling external serialized strings to inspect validation results without throwing uncaught exceptions.
- **R-ZOD-004** MUST: Ensure dynamic default values utilize parse-time transform methods rather than static default initializers to prevent shared state or stale timestamps across validation runs.

### Verify

```bash
# Discover and execute the project repository test runner covering boundary schema validation and parsing contracts
# Discover and execute project repository static analysis verification tools to ensure type checking passes
```

**Accept when:**
- All external ingestion payloads and client configuration objects pass validation against their respective Zod schemas with zero unhandled schema parse exceptions.
- Audit log event schemas enforce discriminated string literal action types with matching structured metadata properties across test suites.
- Dynamic timestamp attributes evaluate at invocation time rather than schema module initialization time.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration build failures will occur if boundary endpoints ingest unvalidated external payloads or lack matching schema declarations.
</enforcement>