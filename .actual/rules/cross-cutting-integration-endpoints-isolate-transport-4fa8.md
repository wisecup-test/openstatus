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

- **R-AST-001** SHOULD: Integration endpoints SHOULD isolate transport-specific formatting into dedicated presenter modules driven by standard tool execution outputs.
- **R-AST-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository (DISCOVERY POLICY).
- **R-AST-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run for version-sensitive behavior at point of use (LOCK-VERSION GROUNDING).
- **R-AST-004** MANDATORY: When exposing new domain capabilities, define the tool specifications and associated validation schemas in the shared services library prior to implementing integration adapters.
- **R-AST-005** MANDATORY: Ensure transport adapters register tool handlers into in-memory dispatch registries and delegate input parsing strictly to the declared schemas.

### Verify

```bash
# Discover the project workspace configuration and execute the static analysis script to verify that all agent tool imports resolve to @openstatus/services/agent-tools.
# Discover and run the project test suite to verify input and output schema validation handling across all registered agent tools.
```

**Accept when:**
- All external tool endpoints and adapters resolve domain tool definitions exclusively from @openstatus/services/agent-tools.
- Static analysis and automated test suites confirm schema validation is enforced on all tool inputs and outputs without local bypasses.

<enforcement>
Verified by: Continuous integration pipelines executing static import verification and lint rules, automated unit and integration test suites validating schema compliance, and mandatory peer code review for all changes to integration routes and adapters.
Violation handling: Pull requests containing local tool declarations or bypassing centralized schemas will fail automated checks and be blocked from merging.
Exception process: Exceptions require formal architectural review and documented justification submitted to the systems architecture group.
</enforcement>