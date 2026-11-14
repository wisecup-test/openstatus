---
version: 2
name: cross-cutting-domain-tool-schemas-execution-handlers-39c7
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Domain Tool Schemas Execution Handlers Not"
metadata:
  actual:
    adr_id: 39c768cc-19d0-4f3d-8d00-fc5a5c3b4edb
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Domain Tool Schemas Execution Handlers Not

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

### Rules

- **R-TOOL-001** MUST_NOT: Domain tool schemas and execution handlers MUST NOT be declared locally within protocol-specific route handlers or presenter modules.

### Verify

```bash
# Discover workspace configuration and execute static analysis to verify agent tool imports resolve to @openstatus/services/agent-tools
# Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>