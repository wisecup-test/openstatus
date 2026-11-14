---
version: 2
name: cross-cutting-service-boundaries-decouple-internal-dat-0c77
description: "@chronark/zod-bird Service Boundary Schema Integration: Service Boundaries Decouple Internal Database Entity"
metadata:
  actual:
    adr_id: 0c77c55b-9e3a-4818-8f18-3926106fd956
    status: accepted
    candidate_group_id: 345a5dc7-c8cf-42dc-aa20-3d73478b08a6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @chronark/zod-bird Service Boundary Schema Integration: Service Boundaries Decouple Internal Database Entity

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for ingestion pipelines and scripts communicating across service boundaries between internal databases and external analytics datasources, and schema migration workflows involving versioned external service endpoints.

### Rules

- **R-ZOD-001** MUST: Service boundaries MUST decouple internal database entity keys from public analytics identifiers through explicit mapping lookups before publishing events.

### Verify

```bash
# Discover and run the project type-checking and static analysis verification scripts across service boundary modules.
# Discover and execute test suites validating schema transformations and service boundary client serialization.
```

**Accept when:**
- Type checking passes without errors across all service boundary schema definitions.
- Payload validation tests verify that malformed egress records fail prior to network dispatch.
- Endpoint migration logic correctly handles existing record thresholds and halts execution when unexpected records are detected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing repository test suites and type verification, and peer code review for all modifications to service boundary schemas and analytics client integrations.
</enforcement>