---
version: 2
name: cross-cutting-consumer-not-bypass-schema-validation-7a8d
description: "@chronark/zod-bird Service Boundary Schema Integration: Consumer Not Bypass Schema Validation Credential"
metadata:
  actual:
    adr_id: 7a8d807e-3481-4812-9d9f-b15a8cf1e4b6
    status: accepted
    candidate_group_id: 345a5dc7-c8cf-42dc-aa20-3d73478b08a6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @chronark/zod-bird Service Boundary Schema Integration: Consumer Not Bypass Schema Validation Credential

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for ingestion pipelines, scripts communicating across service boundaries between internal databases and external analytics datasources, and schema migration workflows involving versioned external service endpoints.

### Rules

- **R-ZOD-001** MUST_NOT: The consumer MUST_NOT bypass schema validation or credential parsing when transferring records across datasource versions.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>