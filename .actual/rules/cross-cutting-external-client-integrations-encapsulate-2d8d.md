---
version: 2
name: cross-cutting-external-client-integrations-encapsulate-2d8d
description: "Twilio Messages API Direct Integration: External Client Integrations Encapsulate Provider Endpoint"
metadata:
  actual:
    adr_id: 2d8da969-c050-4102-b09a-c1ad3307ae73
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: External Client Integrations Encapsulate Provider Endpoint

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for outbound SMS notification dispatch modules interacting with external telephony providers.

### Rules

- **R-EXT-001** SHOULD: External client integrations encapsulate provider endpoint interactions within dedicated boundary modules rather than exposing raw network calls to calling domains.

### Verify

```bash
# Discover and run the project static analysis and test validation suites from the repository root
# Discover and execute contract tests verifying outbound HTTP request serialization against external provider specifications
```

**Accept when:**
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline checks and peer code reviews enforce these requirements.
</enforcement>