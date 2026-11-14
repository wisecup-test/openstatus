---
version: 2
name: cross-cutting-inbound-conversational-payloads-stored-m-8b4f
description: "Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Inbound Conversational Payloads Stored Message Structures"
metadata:
  actual:
    adr_id: 8b4ffdf1-3af9-4ec5-bfb8-381bf489d0bc
    status: accepted
    candidate_group_id: 728cb320-99d6-4631-95b6-9015d5af629e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/api/chat/**
      - src/components/chat/**
      - src/lib/agent-tools/**
---

# Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Inbound Conversational Payloads Stored Message Structures

Scope: applies to `apps/dashboard/src/app/api/chat/**`, `apps/dashboard/src/components/chat/**`, `apps/dashboard/src/lib/agent-tools/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AI-INB-001** MUST: All inbound conversational payloads and stored message structures processed through the ai library MUST be validated against strict schemas before execution or state persistence.

### Verify

```bash
# Discover project verification scripts from the repository manifest and execute the test suite
# Discover and run static analysis checks across modules consuming the conversational library
```

**Accept when:**
- Static type analysis and schema validations pass across all conversational endpoints and UI components.
- Automated tests verify that agent tool adapters successfully parse valid inputs and reject malformed schemas.
- Conversational interactions stream messages and persist session states without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>