---
version: 2
name: cross-cutting-protocol-tool-handlers-not-bypass-e1a7
description: "@modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Protocol Tool Handlers Not Bypass Schema"
metadata:
  actual:
    adr_id: e1a72f6b-742f-4338-840f-ce1f692868fd
    status: accepted
    candidate_group_id: 8a575d33-76df-41a9-b37f-156ab1a4407a
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/**
---

# @modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Protocol Tool Handlers Not Bypass Schema

Scope: applies to `apps/server/src/routes/mcp/**`.

These rules are ALWAYS ACTIVE for all server endpoints and modules that implement Model Context Protocol interfaces or expose agent-facing tools.

### Rules

- **R-MCP-001** MUST_NOT: Protocol tool handlers MUST NOT bypass schema validation or register tools directly with protocol instances without going through the registry adapter interface.

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
Claude Code MUST NOT skip or defer verification. All protocol server tool registrations must pass schema validation checks.
</enforcement>