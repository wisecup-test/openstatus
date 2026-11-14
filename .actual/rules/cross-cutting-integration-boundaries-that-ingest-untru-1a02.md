---
version: 2
name: cross-cutting-integration-boundaries-that-ingest-untru-1a02
description: "Adoption of Zod for Runtime Input and Boundary Validation: Integration Boundaries That Ingest Untrusted Serialized"
metadata:
  actual:
    adr_id: 1a027e31-71d1-4100-a4e6-a5914dbb4a3b
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Integration Boundaries That Ingest Untrusted Serialized

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all integration boundaries that ingest untrusted serialized data, read environment configuration, or construct external network payloads.

### Rules

- **R-ZOD-001** MUST: All integration boundaries that ingest untrusted serialized data, read environment configuration, or construct external network payloads MUST validate data structures using Zod schemas prior to consumption or dispatch.

### Verify

```bash
# Discover the project dependency manifest and lock artifact to identify the registered test and typecheck scripts, then execute the suite to verify that all boundary schemas compile without errors.
# Discover and run the project static analysis and linting verification suite to ensure all input ingestion boundaries invoke Zod schema parse methods on serialized payloads.
# Execute the project automated test suite to validate that malformed input payloads trigger expected validation failures and prevent execution of external client dispatches.
```

**Accept when:**
- All boundary handlers parsing serialized incoming data or configuring external client dispatches pass automated test suites and reject malformed schemas with validation errors.
- Static analysis and typecheck suites complete with zero errors across all modules declaring and parsing Zod boundary schemas.
- The resolved library version matches the documented API usage confirmed from the authoritative dependency lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>