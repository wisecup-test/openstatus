---
version: 2
name: cross-cutting-consumers-versioned-dependencies-discove-013a
description: "Adoption of @openstatus/notification-base for Notification Integrations: Consumers Versioned Dependencies Discover Ecosystem Lock"
metadata:
  actual:
    adr_id: 013afc2a-51c6-4186-a1bc-acb250c36ed7
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Consumers Versioned Dependencies Discover Ecosystem Lock

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, provider delivery modules implementing outbound alerting or messaging, and components handling incoming notification dispatch events requiring payload parsing and external service delivery.

### Rules

- **R-NB-001** MUST: Consumers of versioned dependencies MUST discover the ecosystem lock artifact and inspect resolved dependency declarations to enforce exact version conformance prior to integration.

### Verify

```bash
# Discover and run the repository build validation script to ensure type-checking and interface conformance with the shared base package.
# Discover and execute the project test runner to validate notification dispatch handling and payload transformation behavior.
# Discover and execute the workspace linter to confirm adherence to package boundary and import restrictions.
```

**Accept when:**
- All notification provider modules successfully compile and satisfy interface contracts defined in @openstatus/notification-base.
- External dispatch execution tests pass with valid payload assertions and schema validation checks.
- Dependency boundaries and package resolution checks succeed without unresolved imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline runs repository build and test discovery scripts on pull requests. Pull requests bypassing shared base contracts or introducing unvalidated external client dispatches will be blocked from merge.
</enforcement>