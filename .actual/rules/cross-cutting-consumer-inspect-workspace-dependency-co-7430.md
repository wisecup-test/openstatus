---
version: 2
name: cross-cutting-consumer-inspect-workspace-dependency-co-7430
description: "Adopt @openstatus/notification-base for Notification Providers: Consumer Inspect Workspace Dependency Configuration Lock"
metadata:
  actual:
    adr_id: 7430ec8b-53fa-4c04-8084-59b10d7f52fa
    status: accepted
    candidate_group_id: ab96645c-553b-4d83-8872-9ea43f28fec3
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Adopt @openstatus/notification-base for Notification Providers: Consumer Inspect Workspace Dependency Configuration Lock

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: The consumer MUST inspect the workspace dependency configuration and lock resolution artifacts to resolve the exact dependency version before integrating @openstatus/notification-base.

### Verify

```bash
# Discover and run the project workspace type-check script to verify module interface compatibility.
# Discover and run the test suite for notification provider packages to validate delivery dispatch behavior.
# Discover and run the repository linter to verify dependency boundary compliance across internal packages.
```

**Accept when:**
- The notification provider compiles with zero type errors when referencing @openstatus/notification-base.
- Payload validation succeeds for conformant data and rejects invalid inputs prior to external network requests.
- All discovered package test suites pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>