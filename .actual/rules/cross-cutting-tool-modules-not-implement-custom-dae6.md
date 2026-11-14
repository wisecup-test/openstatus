---
version: 2
name: cross-cutting-tool-modules-not-implement-custom-dae6
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Modules Not Implement Custom Protocol"
metadata:
  actual:
    adr_id: dae670ec-9b7d-42cb-b43b-cdf44cac5b12
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Modules Not Implement Custom Protocol

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for tool handler modules exposing server domain capabilities through the Model Context Protocol and modules integrating internal domain services with external agent execution environments.

### Rules

- **R-MCP-001** SHOULD_NOT: Tool modules SHOULD NOT implement custom protocol serialization formats outside the structures provided by @modelcontextprotocol/sdk.

### Verify

```bash
# Discover the project test execution script from the repository manifest and execute the test suite for tool route handlers.
# Discover the project static analysis and linting scripts from the repository manifest and execute them across the server codebase.
# Discover the project build and type checking scripts from the repository manifest and verify compilation across all protocol integration endpoints.
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing repository test and type validation suites and peer code review verifying adherence to protocol adapter isolation patterns.
</enforcement>