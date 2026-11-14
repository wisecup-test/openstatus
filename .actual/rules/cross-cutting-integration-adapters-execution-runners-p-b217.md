---
version: 2
name: cross-cutting-integration-adapters-execution-runners-p-b217
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Integration Adapters Execution Runners Perform Input"
metadata:
  actual:
    adr_id: b217f14c-5e66-4a63-b4b1-e935aa8c9231
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Integration Adapters Execution Runners Perform Input

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

### Rules

- **R-AT-001** MUST: Integration adapters and execution runners MUST perform input schema validation using the tool inputSchema contract prior to invoking the underlying service logic.

### Verify

```bash
# Discover the project workspace configuration and execute the static analysis script to verify that all agent tool imports resolve to @openstatus/services/agent-tools.
# Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools.
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes to integration routes and adapters must be verified by automated tests, static analysis, and peer code review.
</enforcement>