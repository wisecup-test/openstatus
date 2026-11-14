---
version: 2
name: cross-cutting-protocol-tool-handlers-delegate-underlyi-3cd1
description: "@modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Protocol Tool Handlers Delegate Underlying Business"
metadata:
  actual:
    adr_id: 3cd186ad-dcfa-49fa-b157-23513ab0e9e8
    status: accepted
    candidate_group_id: 8a575d33-76df-41a9-b37f-156ab1a4407a
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/**
---

# @modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Protocol Tool Handlers Delegate Underlying Business

Scope: applies to `apps/server/src/routes/mcp/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MCP-001** SHOULD: Protocol tool handlers SHOULD delegate underlying business logic directly to domain service modules rather than implementing domain logic within the protocol tool definitions.

### Verify

```bash
# Discover and run project static type inspection script
# Discover and run test execution suite to validate tool registration and input/output schema parsing
# Discover and run repository linter
```

**Accept when:**
- All protocol server tool registrations pass schema validation checks with zero type errors.
- Automated test suites verify that invalid tool inputs are rejected before reaching domain services.
- Protocol tool execution returns properly structured and validated output envelopes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>