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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: All external telemetry payloads, audit log events, and client configuration inputs crossing external boundaries MUST be validated using Zod runtime schemas before consumption or datastore ingestion.
- **R-ZOD-002** MUST: Derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MUST: Execute lock-version grounding before writing code that uses a versioned library by finding the manifest, identifying the build tool, inspecting the lock/resolution artifact, looking up official documentation for the exact version, and confirming API existence.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>