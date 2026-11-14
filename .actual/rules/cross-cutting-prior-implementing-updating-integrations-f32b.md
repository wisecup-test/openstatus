---
version: 2
name: cross-cutting-prior-implementing-updating-integrations-f32b
description: "Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Prior Implementing Updating Integrations That Depend"
metadata:
  actual:
    adr_id: f32b4518-8b60-4e59-ae92-fdce31333b88
    status: accepted
    candidate_group_id: 9bd500d7-caf0-4fe0-ae1e-44d97d6b6b6c
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Prior Implementing Updating Integrations That Depend

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for outbound notification handler modules that adapt internal notification payloads into external client requests.

### Rules

- **R-NOTIF-001** MUST: Prior to implementing or updating integrations that depend on external or shared packages, developers MUST discover the dependency declaration artifact and resolve the exact locked version from the repository resolution artifact.
- **R-NOTIF-002** MUST: Execute version grounding steps (find manifest, identify build tool, inspect lockfile, check documentation, and confirm APIs) before writing code that uses a versioned library.
- **R-NOTIF-003** MUST: Parse notification payloads through data schemas to verify data integrity prior to external network dispatch.
- **R-NOTIF-004** MUST: Encapsulate external network client fetch invocations in comprehensive error handling that logs failure diagnostics without halting unrelated worker processes.

### Verify

```bash
# Discover and run the project's static analysis and type-checking scripts from build configuration
# Discover and execute the project's unit and integration test suite to validate notification parsing and webhook dispatch logic
```

**Accept when:**
- Incoming notification payloads pass schema validation before triggering network calls.
- Outbound HTTP POST requests successfully serialize JSON content to target webhook endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated build pipeline checks and peer code reviews enforce compliance.
</enforcement>