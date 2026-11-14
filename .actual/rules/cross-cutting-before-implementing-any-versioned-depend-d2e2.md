---
version: 2
name: cross-cutting-before-implementing-any-versioned-depend-d2e2
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Before Implementing Any Versioned Dependency Consumers"
metadata:
  actual:
    adr_id: d2e26613-716b-4024-b24e-78270e661490
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Before Implementing Any Versioned Dependency Consumers

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

### Rules

- **R-AGENT-001** MUST: Before implementing any versioned dependency, consumers MUST discover the project dependency manifest and repository lock artifact to verify the exact resolved package version.
- **R-AGENT-002** MUST: Define tool specifications and associated validation schemas in the shared services library prior to implementing integration adapters when exposing new domain capabilities.
- **R-AGENT-003** MUST: Ensure transport adapters register tool handlers into in-memory dispatch registries and delegate input parsing strictly to the declared schemas.

### Verify

```bash
# Discover the project workspace configuration and execute static analysis to verify agent tool imports resolve to @openstatus/services/agent-tools
# Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external tool endpoints and adapters must strictly adhere to centralized schema definitions and verified package dependencies.
</enforcement>