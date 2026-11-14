---
version: 2
name: cross-cutting-server-application-use-modelcontextproto-6566
description: "@modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Server Application Use Modelcontextprotocol Sdk Mcp"
metadata:
  actual:
    adr_id: 65660960-499c-40aa-a1f5-5fa661a66dd9
    status: accepted
    candidate_group_id: 8a575d33-76df-41a9-b37f-156ab1a4407a
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/**
---

# @modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Server Application Use Modelcontextprotocol Sdk Mcp

Scope: applies to `apps/server/src/routes/mcp/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MCP-001** MUST: The server application MUST use @modelcontextprotocol/sdk/server/mcp.js as the standard protocol server implementation for all agent tool registrations and protocol communication endpoints.

### Verify

```bash
# Discover and run the project static type inspection script to verify type adherence across protocol modules.
# Discover and run the test execution suite to validate tool registration and input and output schema parsing behavior.
# Discover and run the repository linter to ensure consistency and adherence to structural conventions.
```

**Accept when:**
- All protocol server tool registrations pass schema validation checks with zero type errors.
- Automated test suites verify that invalid tool inputs are rejected before reaching domain services.
- Protocol tool execution returns properly structured and validated output envelopes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>