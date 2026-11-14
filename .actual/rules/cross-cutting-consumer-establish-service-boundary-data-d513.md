---
version: 2
name: cross-cutting-consumer-establish-service-boundary-data-d513
description: "@chronark/zod-bird Service Boundary Schema Integration: Consumer Establish Service Boundary Data Contracts"
metadata:
  actual:
    adr_id: d5135190-f613-4ca0-9ce4-94537f66c1af
    status: accepted
    candidate_group_id: 345a5dc7-c8cf-42dc-aa20-3d73478b08a6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @chronark/zod-bird Service Boundary Schema Integration: Consumer Establish Service Boundary Data Contracts

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SB-001** MUST: The consumer MUST establish service boundary data contracts for external analytics ingestion by validating all egress payloads against schemas defined via @chronark/zod-bird.

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