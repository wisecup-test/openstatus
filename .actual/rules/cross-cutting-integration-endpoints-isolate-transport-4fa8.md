---
version: 2
name: cross-cutting-integration-endpoints-isolate-transport-4fa8
description: "Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Integration Endpoints Isolate Transport Specific Formatting"
metadata:
  actual:
    adr_id: 4fa8ba68-487f-4ed0-9c46-21f63a86bcdc
    status: accepted
    candidate_group_id: 247ddcd8-2920-4fe9-8d3e-4d2918442089
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/services/agent-tools for Centralized Agent Tool Definitions: Integration Endpoints Isolate Transport Specific Formatting

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route handlers, protocol adapters, runners, and presenters exposing domain capabilities to external agents and conversational integrations.

### Rules

- **R-AGENT-001** SHOULD: Integration endpoints isolate transport-specific formatting into dedicated presenter modules driven by standard tool execution outputs.
- **R-AGENT-002** MANDATORY: When exposing new domain capabilities, define the tool specifications and associated validation schemas in the shared services library prior to implementing integration adapters.
- **R-AGENT-003** MANDATORY: Ensure transport adapters register tool handlers into in-memory dispatch registries and delegate input parsing strictly to the declared schemas.
- **R-AGENT-004** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-AGENT-005** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up the official documentation, changelog, or public API reference for that exact version. (5) Confirm every API, class, or function you will call exists in that exact version's documentation before using it. (6) For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover the project workspace configuration and execute static analysis to verify agent tool imports resolve to the shared package
# Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, automated test suites, and peer code reviews enforce compliance.
</enforcement>