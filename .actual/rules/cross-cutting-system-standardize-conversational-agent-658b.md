---
version: 2
name: cross-cutting-system-standardize-conversational-agent-658b
description: "Adoption of the ai Library for Conversational Interfaces and Agent Workflows: System Standardize Conversational Agent Interactions Streaming"
metadata:
  actual:
    adr_id: 658ba801-a8e3-4c9b-8b75-a50d457e4a3c
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

# Adoption of the ai Library for Conversational Interfaces and Agent Workflows: System Standardize Conversational Agent Interactions Streaming

Scope: applies to `apps/dashboard/src/app/api/chat/**`, `apps/dashboard/src/components/chat/**`, `apps/dashboard/src/lib/agent-tools/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AI-001** MUST: Standardize all conversational agent interactions, streaming message protocols, and tool execution adapters on the adopted ai core library across both server and client domains.

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