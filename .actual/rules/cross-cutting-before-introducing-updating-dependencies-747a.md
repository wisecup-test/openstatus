---
version: 2
name: cross-cutting-before-introducing-updating-dependencies-747a
description: "Twilio Messages API Direct Integration: Before Introducing Updating Dependencies That Integrate"
metadata:
  actual:
    adr_id: 747a774b-59e1-4831-a911-bd64dbe50596
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: Before Introducing Updating Dependencies That Integrate

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for outbound SMS notification dispatch modules interacting with external telephony providers.

### Rules

- **R-TW-001** MUST: Before introducing or updating dependencies that integrate with external communication providers, consumers MUST inspect the repository lock artifact to determine the exact resolved dependency versions.
- **R-TW-002** MANDATORY: Execute the lock-version grounding process in order (find manifest, identify build tool, inspect lock artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-TW-003** MANDATORY: Ensure outbound authorization headers and credentials are loaded exclusively through validated environment configurations.
- **R-TW-004** MANDATORY: Construct URL-encoded request bodies using standard key-value serialization matching external provider parameter specifications.

### Verify

```bash
# Discover and run the project static analysis and test validation suites from the repository root
# Discover and execute contract tests verifying outbound HTTP request serialization against external provider specifications
```

**Accept when:**
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>