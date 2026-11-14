---
version: 2
name: cross-cutting-tool-definitions-decouple-domain-busines-dd75
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Definitions Decouple Domain Business Logic"
metadata:
  actual:
    adr_id: dd7517e4-12e6-4fb5-984e-e40a9de8cc03
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Definitions Decouple Domain Business Logic

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MCP-001** MUST: Tool definitions MUST decouple domain business logic from the protocol transport layer by mediating domain service invocations through an internal registry adapter.

### Verify

```bash
# Discover and execute project test suite for tool route handlers from the repository manifest
# Discover and execute project static analysis and linting scripts from the repository manifest across the server codebase
# Discover and execute project build and type checking scripts from the repository manifest to verify compilation
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews verify adherence to protocol adapter isolation patterns.
</enforcement>