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

These rules are ALWAYS ACTIVE for notification adapters, dispatch modules, and runtime credential resolution handling outbound notification requests.

### Rules

- **R-SEC-001** SHOULD: Outbound external client requests transmit authentication tokens via authorization headers rather than request path parameters where supported by the third-party endpoint.
- **R-SEC-002** MANDATORY: Verify that all required environment variables are defined in the runtime deployment configuration prior to service initialization.
- **R-SEC-003** MANDATORY: Ensure outbound request builders sanitize sensitive parameters before emitting diagnostic traces.
- **R-SEC-004** MANDATORY: Follow the LOCK-VERSION GROUNDING sequence (find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use) before writing code that uses a versioned library.

### Verify

```bash
# Discover and run the project's static analysis and test validation suites to verify secret handling and input schema compliance.
# Execute the repository code scanner to ensure no hardcoded secrets or exposed tokens exist in source files.
```

**Accept when:**
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests violating secret handling or schema validation rules will be blocked from merging until corrected.
</enforcement>