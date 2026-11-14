---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-9be9
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Engineers Discover Repository Dependency Lock File"
metadata:
  actual:
    adr_id: 9be97fb7-8077-494b-9459-502f7036380b
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Engineers Discover Repository Dependency Lock File

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for tool handler modules exposing server domain capabilities through the Model Context Protocol and modules integrating internal domain services with external agent execution environments.

### Rules

- **R-MCP-001** MUST: Engineers MUST discover the repository dependency lock file and verify that the exact resolved version of @modelcontextprotocol/sdk satisfies compatibility requirements before introducing or updating tool endpoints.
- **R-MCP-002** MUST: Follow lock-version grounding: find the dependency manifest, identify the build tool, inspect the lock/resolution artifact for the exact resolved version, fetch version-specific documentation for that exact version, and confirm every API/class/function exists before using it.
- **R-MCP-003** MUST: Implement new agent tools as dedicated domain modules that register capabilities through the shared registry adapter rather than binding directly to protocol server instances.
- **R-MCP-004** MUST: Ensure all tool input and output structures define formal schemas compatible with the underlying protocol SDK capabilities.

### Verify

```bash
# Discover and execute project test script for tool route handlers from the manifest
# Discover and execute project static analysis and linting scripts from the manifest
# Discover and execute project build and type checking scripts from the manifest
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing direct domain coupling or bypassing the protocol SDK must be rejected during review, and build failures resulting from type mismatches or unverified dependency versions must block merge.
</enforcement>