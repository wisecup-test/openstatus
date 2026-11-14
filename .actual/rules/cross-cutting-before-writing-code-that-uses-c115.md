---
version: 2
name: cross-cutting-before-writing-code-that-uses-c115
description: "@chronark/zod-bird Service Boundary Schema Integration: Before Writing Code That Uses Versioned"
metadata:
  actual:
    adr_id: c115be99-06c2-4410-b0b4-11f98987b90c
    status: accepted
    candidate_group_id: 345a5dc7-c8cf-42dc-aa20-3d73478b08a6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @chronark/zod-bird Service Boundary Schema Integration: Before Writing Code That Uses Versioned

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for ingestion pipelines and scripts communicating across service boundaries between internal databases and external analytics datasources, and schema migration workflows involving versioned external service endpoints.

### Rules

- **R-ZOD-001** MUST: Before writing code that uses a versioned library, the consumer MUST inspect the repository lock or resolution artifact to determine and verify the exact resolved version.

### Verify

```bash
# Discover and run the project type-checking and static analysis verification scripts across service boundary modules.
npm run typecheck
npm run test
```

**Accept when:**
- Type checking passes without errors across all service boundary schema definitions.
- Payload validation tests verify that malformed egress records fail prior to network dispatch.
- Endpoint migration logic correctly handles existing record thresholds and halts execution when unexpected records are detected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository test suites and type verification, and peer code review is required for all modifications to service boundary schemas and analytics client integrations.
</enforcement>