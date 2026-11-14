---
version: 2
name: cross-cutting-developers-discover-project-dependency-r-f9e0
description: "Adoption of @openstatus/notification-base for Notification Modules: Developers Discover Project Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: f9e0ea22-c30e-4f3f-a2a9-5109eed4522c
    status: accepted
    candidate_group_id: c9667bbc-a93e-4be6-8512-d4d9f3810ba7
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Modules: Developers Discover Project Dependency Resolution Artifact

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all notification integration modules and dispatchers within the notification subsystem.

### Rules

- **R-NOTIF-001** MUST: Developers MUST discover the project dependency resolution artifact to verify the locked version of @openstatus/notification-base before implementing consuming modules.

### Verify

```bash
# Discover and execute repository dependency boundary and module linting scripts
# Discover and run test suites targeting notification provider modules
```

**Accept when:**
- Static analysis confirms notification integration modules import shared notification primitives from @openstatus/notification-base.
- All automated test suites and contract validations for notification delivery pass without structural dependency violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration dependency checks and automated module boundary validation rules enforce these requirements.
</enforcement>