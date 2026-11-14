---
version: 2
name: cross-cutting-when-resolving-dependencies-internal-sch-dfa8
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: When Resolving Dependencies Internal Schema Module"
metadata:
  actual:
    adr_id: dfa800f4-9e80-4528-b9f5-3fc195eb2861
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: When Resolving Dependencies Internal Schema Module

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for notification integration packages, mock utilities, test fixtures, and testing harnesses simulating notification data structures.

### Rules

- **R-SCHEMA-001** MUST: When resolving dependencies for the internal schema module, consumers MUST discover the project manifest and lock artifact to verify dependency resolution consistency before implementation.
- **R-SCHEMA-002** MUST: When introducing new notification entity structures, export the corresponding schemas from @openstatus/db/src/schema prior to consumption in notification integration modules.
- **R-SCHEMA-003** MUST: Ensure mock utilities and test fixture generators reference exported schema definitions from @openstatus/db/src/schema to preserve contract parity with runtime handlers.

### Verify

```bash
# Discover the workspace configuration to locate validation scripts and run the static type checker across notification packages.
# Discover and execute the module dependency linting script to verify that database schema imports originate solely from @openstatus/db/src/schema.
# Execute the discovered test execution script for notification packages to verify mock harness compatibility against schema definitions.
```

**Accept when:**
- All notification service modules and mock harnesses resolve schema imports from @openstatus/db/src/schema with zero compilation or type resolution errors.
- Static dependency boundary checks verify that no duplicate or shadowed database schema definitions exist within notification packages.
- Automated test suites for notification services and mock utilities execute successfully using the canonical schema contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, code reviews, and build scripts verify static types, import boundaries, and canonical schema contracts.
</enforcement>