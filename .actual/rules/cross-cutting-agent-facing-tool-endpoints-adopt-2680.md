---
version: 2
name: cross-cutting-agent-facing-tool-endpoints-adopt-2680
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Agent Facing Tool Endpoints Adopt Modelcontextprotocol"
metadata:
  actual:
    adr_id: 26801491-f02e-469d-8eac-f18dc8574810
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Agent Facing Tool Endpoints Adopt Modelcontextprotocol

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MCP-001** MUST: All agent-facing tool endpoints MUST adopt @modelcontextprotocol/sdk/server/mcp.js as the standardized interface boundary for defining and registering server tools.

### Verify

```bash
# Discover and execute project test execution script for tool route handlers
# Discover and execute project static analysis and linting scripts across the server codebase
# Discover and execute project build and type checking scripts across protocol integration endpoints
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository test and type validation suites, and peer code review verifies adherence to protocol adapter isolation patterns. Pull requests introducing direct domain coupling or bypassing the protocol SDK must be rejected during review.
</enforcement>