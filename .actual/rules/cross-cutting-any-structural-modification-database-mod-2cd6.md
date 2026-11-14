---
version: 2
name: cross-cutting-any-structural-modification-database-mod-2cd6
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Any Structural Modification Database Models Consumed"
metadata:
  actual:
    adr_id: 2cd69fa8-c9d4-4d10-8a29-1fe64b7f950c
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Any Structural Modification Database Models Consumed

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, mock utilities, test fixtures, and testing harnesses consuming database entities or notification payloads.

### Rules

- **R-DB-001** SHOULD: Any structural modification to database models consumed by notification services SHOULD be introduced and exported within @openstatus/db/src/schema before updating consuming modules.

### Verify

```bash
# Discover the workspace configuration to locate validation scripts and run the static type checker across notification packages
# Discover and execute the module dependency linting script to verify that database schema imports originate solely from @openstatus/db/src/schema
# Execute the discovered test execution script for notification packages to verify mock harness compatibility against schema definitions
```

**Accept when:**
- All notification service modules and mock harnesses resolve schema imports from @openstatus/db/src/schema with zero compilation or type resolution errors.
- Static dependency boundary checks verify that no duplicate or shadowed database schema definitions exist within notification packages.
- Automated test suites for notification services and mock utilities execute successfully using the canonical schema contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Build and verification scripts will reject modules declaring local duplicates of database schema structures.
</enforcement>