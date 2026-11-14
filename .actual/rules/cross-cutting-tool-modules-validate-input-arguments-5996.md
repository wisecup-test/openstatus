---
version: 2
name: cross-cutting-tool-modules-validate-input-arguments-5996
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Modules Validate Input Arguments Against"
metadata:
  actual:
    adr_id: 59963acf-1334-441a-8c7a-d89bac4b133b
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Modules Validate Input Arguments Against

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for tool handler modules exposing server domain capabilities through the Model Context Protocol and modules integrating internal domain services with external agent execution environments.

### Rules

- **R-MCP-001** SHOULD: Tool modules validate input arguments against strict schemas before delegating execution to underlying service operations.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository test and type validation suites, and peer code reviews verify adherence to protocol adapter isolation patterns.
</enforcement>