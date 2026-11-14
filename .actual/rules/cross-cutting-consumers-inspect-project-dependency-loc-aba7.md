---
version: 2
name: cross-cutting-consumers-inspect-project-dependency-loc-aba7
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Consumers Inspect Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: aba72ede-6983-4b45-9d1c-f312f594abf1
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Consumers Inspect Project Dependency Lock Artifact

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SUB-001** MUST: Consumers MUST inspect the project dependency lock artifact to confirm the exact resolved version and exported interface of @openstatus/services/page-subscriber prior to introducing or modifying subscriber integrations.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify interface compatibility with the subscriber service module.
# Discover and run the project test suite covering router endpoints and domain service integration.
```

**Accept when:**
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>