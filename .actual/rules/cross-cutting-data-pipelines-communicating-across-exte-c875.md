---
version: 2
name: cross-cutting-data-pipelines-communicating-across-exte-c875
description: "@chronark/zod-bird Service Boundary Schema Integration: Data Pipelines Communicating Across External Service"
metadata:
  actual:
    adr_id: c875a0c1-0ced-4fe3-87fa-9b61feeb69c4
    status: accepted
    candidate_group_id: 345a5dc7-c8cf-42dc-aa20-3d73478b08a6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @chronark/zod-bird Service Boundary Schema Integration: Data Pipelines Communicating Across External Service

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for ingestion pipelines, scripts, and service boundary schema implementations communicating between internal databases and external analytics datasources.

### Rules

- **R-SB-001** SHOULD: Data pipelines communicating across external service boundaries verify existing record counts and prevent duplicate ingestion unless an explicit override is provided.

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