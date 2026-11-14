---
version: 2
name: cross-cutting-serialized-external-data-payloads-deseri-710b
description: "Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Serialized External Data Payloads Deserialized Verified"
metadata:
  actual:
    adr_id: 710bb423-b592-4177-9202-2ab2dc74e3fd
    status: accepted
    candidate_group_id: e31ac072-3db1-4961-a5ee-464d55ca9a14
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Serialized External Data Payloads Deserialized Verified

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all external data ingestion interfaces, telemetry schemas, audit log event contracts, and analytics client configuration parsing.

### Rules

- **R-ZOD-001** SHOULD: Serialized external data payloads SHOULD be deserialized and verified using safe parsing methods that return structured success or error objects rather than throwing unhandled runtime exceptions.
- **R-ZOD-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence: find the dependency manifest, identify the build tool, inspect the lock or resolution artifact for exact resolved versions, look up official documentation for that exact version, confirm every API/class/function exists, and re-run per dependency at point of use.
- **R-ZOD-004** MANDATORY: Compose granular reusable base schemas into higher-level action schemas using object extension and literal discriminators.
- **R-ZOD-005** MANDATORY: Utilize safe parsing wrappers when handling external serialized strings to inspect validation results without throwing uncaught exceptions.

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
Claude Code MUST NOT skip or defer verification. Continuous integration build failures occur when boundary endpoints ingest unvalidated external payloads or lack matching schema declarations. Mandatory code revision requests are enforced during review for any boundary data access bypassing schema parsing.
</enforcement>