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

- **R-MCP-001** SHOULD_NOT: Tool modules MUST NOT implement custom protocol serialization formats outside the structures provided by @modelcontextprotocol/sdk.

### Verify

```bash
# Discover and execute project test suite for tool route handlers from package manifest
# Discover and execute static analysis and linting scripts from package manifest
# Discover and execute build and type checking scripts from package manifest
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code review enforce protocol adapter isolation patterns.
</enforcement>