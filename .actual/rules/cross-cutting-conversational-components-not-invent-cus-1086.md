---
version: 2
name: cross-cutting-conversational-components-not-invent-cus-1086
description: "Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Conversational Components Not Invent Custom Message"
metadata:
  actual:
    adr_id: 108692af-3238-44b7-b094-3565271cd08c
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

# Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Conversational Components Not Invent Custom Message

Scope: applies to `apps/dashboard/src/app/api/chat/**`, `apps/dashboard/src/components/chat/**`, `apps/dashboard/src/lib/agent-tools/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONV-001** MUST_NOT: Conversational components MUST NOT invent custom message schema shapes or bespoke streaming protocols that bypass the data structures established by the ai library.

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