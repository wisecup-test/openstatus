---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-f1cf
description: "Adoption of @openstatus/notification-base Core Library: Developers Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: f1cfa6b3-93bd-43c1-a294-e6d7b6624c3d
    status: accepted
    candidate_group_id: 134a167d-ded5-4810-8ca0-70ed27e07461
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base Core Library: Developers Inspect Repository Dependency Resolution Artifact

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Developers MUST inspect the repository dependency resolution artifact to determine the exact resolved version of internal and external notification dependencies before implementing handler logic.

### Verify

```bash
# Discover and execute the test runner command from the repository build manifest
# Discover and execute the workspace type-checking command
# Discover and execute the linting and static analysis command
```

**Accept when:**
- All notification handler test suites execute and pass without schema validation or network delivery errors.
- Static type analysis confirms that all payload transformations conform to provider schema definitions.
- All external HTTP client calls transmit properly validated JSON payloads with correct content headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated CI pipelines and peer code reviews.
</enforcement>