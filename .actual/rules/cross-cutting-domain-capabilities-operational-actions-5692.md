---
version: 2
name: cross-cutting-domain-capabilities-operational-actions-5692
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Domain Capabilities Operational Actions Exposed External"
metadata:
  actual:
    adr_id: 5692591e-1dcd-4a4b-acbe-10a9603c4378
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Domain Capabilities Operational Actions Exposed External

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

### Rules

- **R-EXT-001** MUST: All domain capabilities and operational actions exposed to external agent runtimes and conversational integrations MUST be defined in and imported from the @openstatus/services/agent-tools library.

### Verify

```bash
# Discover the project workspace configuration and execute static analysis/lint verification
# to ensure all agent tool imports resolve to @openstatus/services/agent-tools

# Discover and run the project test suite to verify input and output schema validation
# handling across all registered agent tools
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines execute static import verification, lint rules, and automated unit/integration test suites.
</enforcement>