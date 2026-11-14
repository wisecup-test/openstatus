---
version: 2
name: cross-cutting-protocol-adapters-returning-structured-e-f190
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Protocol Adapters Returning Structured Execution Results"
metadata:
  actual:
    adr_id: f1903a4e-0017-42c0-a2b3-32c4328abf78
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Protocol Adapters Returning Structured Execution Results

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

### Rules

- **R-PROT-001** MUST: Protocol adapters returning structured execution results MUST validate response payloads against the tool outputSchema contract before dispatching responses.

### Verify

```bash
# Discover the project workspace configuration and execute the static analysis script to verify that all agent tool imports resolve to @openstatus/services/agent-tools.
# Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools.
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration pipelines, automated unit and integration tests, and mandatory peer code review.
</enforcement>