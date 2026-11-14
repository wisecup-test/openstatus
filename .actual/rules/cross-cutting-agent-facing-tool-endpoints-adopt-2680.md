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
- **R-MCP-002** MUST: Implement new agent tools as dedicated domain modules that register capabilities through the shared registry adapter rather than binding directly to protocol server instances.
- **R-MCP-003** MUST: Ensure all tool input and output structures define formal schemas compatible with the underlying protocol SDK capabilities.
- **R-MCP-004** MUST: Before writing code that uses a versioned library, execute the LOCK-VERSION GROUNDING procedure: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that exact version, and re-run steps per dependency at point of use for version-sensitive behavior.

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
Claude Code MUST NOT skip or defer verification. All agent-facing tool endpoints must be verified using repository-defined test suites and static analysis checks. Pull requests introducing direct domain coupling or bypassing the protocol SDK must be rejected during review, and build failures resulting from type mismatches or unverified dependency versions must block merge.
</enforcement>