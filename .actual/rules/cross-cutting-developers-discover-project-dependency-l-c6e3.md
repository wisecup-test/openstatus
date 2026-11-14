---
version: 2
name: cross-cutting-developers-discover-project-dependency-l-c6e3
description: "Adoption of @openstatus/notification-base for Notification Channel Modules: Developers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: c6e3cf74-1fd6-491b-8703-ff77c897cc52
    status: accepted
    candidate_group_id: d4bf6f6d-dbc1-4ba6-ab0c-83430fda99ff
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Channel Modules: Developers Discover Project Dependency Lock Artifact

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, channel providers, message block formatters, and dispatch handlers within the notification subsystem.

### Rules

- **R-NB-001** MUST: Developers MUST discover the project dependency lock artifact and verify the exact resolved version of @openstatus/notification-base before introducing changes or consuming its exported interfaces.

### Verify

```bash
# Discover the workspace dependency graph and verify notification channel packages depend on @openstatus/notification-base
# Execute repository static analysis and typecheck verification routines
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all notification channel packages importing @openstatus/notification-base.
- Dependency graph inspection confirms all notification modules resolve @openstatus/notification-base consistently according to the repository resolution artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated workspace dependency boundary checks, static analysis in CI pipelines, and mandatory peer code reviews are enforced.
</enforcement>