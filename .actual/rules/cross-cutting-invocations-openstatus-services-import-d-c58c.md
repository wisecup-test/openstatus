---
version: 2
name: cross-cutting-invocations-openstatus-services-import-d-c58c
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Invocations Openstatus Services Import Domain Specific"
metadata:
  actual:
    adr_id: c58ce9d1-5bd0-4a0a-91dc-15149aa480b1
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Invocations Openstatus Services Import Domain Specific

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedure handlers and domain service integrations across workspace packages interacting with core entities.

### Rules

- **R-OPENSTATUS-001** SHOULD: Invocations of @openstatus/services SHOULD import domain-specific operations directly from granular subpath modules rather than broad monolithic entry points.

### Verify

```bash
# Discover and run the repository test runner script covering API routes and service adapters
# Discover and run workspace static analysis and type checking scripts across affected packages
```

**Accept when:**
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, type verification executed during CI, and architectural review of pull requests.
</enforcement>