---
version: 2
name: cross-cutting-agent-tool-integrations-wrap-underlying-76a4
description: "Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Agent Tool Integrations Wrap Underlying Service"
metadata:
  actual:
    adr_id: 76a493c1-439c-4569-9019-9037c6c21f6c
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

# Adoption of the ai Library for Conversational Interfaces and Agent Workflows: Agent Tool Integrations Wrap Underlying Service

Scope: applies to `apps/dashboard/src/app/api/chat/**`, `apps/dashboard/src/components/chat/**`, `apps/dashboard/src/lib/agent-tools/**`.

These rules are ALWAYS ACTIVE for all server-side endpoints orchestrating conversational chat interactions, streaming responses, agent tool adapters defining executable actions and input parameter schemas, and client-side hooks, state managers, and presentation components rendering conversational messages.

### Rules

- **R-AI-001** SHOULD: Agent tool integrations SHOULD wrap underlying service invocations within structured adapters that expose validated input schemas compatible with the ai library execution interface.
- **R-AI-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-AI-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API exists in that exact version, and re-run steps per dependency at point of use.
- **R-AI-004** MANDATORY: Define agent tools by pairing domain service capabilities with schema definitions that validate incoming execution arguments.
- **R-AI-005** MANDATORY: Handle streaming conversational persistence asynchronously to prevent storage failures from aborting active client chat sessions.

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
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipeline running automated tests and static type checks, and peer code review verifying adherence to library abstractions and schema validation rules.
</enforcement>