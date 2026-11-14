---
version: 2
name: cross-cutting-outbound-external-client-requests-transm-9eb5
description: "@openstatus/notification-base Environment Secrets Handling: Outbound External Client Requests Transmit Authentication"
metadata:
  actual:
    adr_id: 9eb56ee8-5e16-4f8b-ae82-4b07cc36b6b3
    status: accepted
    candidate_group_id: b721e395-9761-4cb0-a0a8-e052244b08ac
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# @openstatus/notification-base Environment Secrets Handling: Outbound External Client Requests Transmit Authentication

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for notification adapters, dispatch modules integrating with external messaging services, and direct runtime credential resolution for outbound notification requests.

### Rules

- **R-NET-001** SHOULD: Outbound external client requests SHOULD transmit authentication tokens via authorization headers rather than request path parameters where supported by the third-party endpoint.
- **R-VAL-001** MANDATORY: Input payloads MUST be parsed and validated prior to external notification dispatch using structured schemas.
- **R-ENV-001** MANDATORY: The consumer MUST discover dependency manifest files, identify build tools, inspect lock or resolution artifacts, and fetch version-specific documentation before writing code that uses a versioned library.
- **R-ENV-002** MANDATORY: Verify that all required environment variables are defined in the runtime deployment configuration prior to service initialization.
- **R-SEC-001** MANDATORY: Ensure outbound request builders sanitize sensitive parameters before emitting diagnostic traces.

### Verify

```bash
# Discover and run the project's static analysis and test validation suites
# Execute the repository code scanner to ensure no hardcoded secrets or exposed tokens exist in source files
```

**Accept when:**
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce these rules, and violations will block merging until corrected.
</enforcement>