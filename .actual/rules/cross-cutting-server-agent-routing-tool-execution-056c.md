---
version: 2
name: cross-cutting-server-agent-routing-tool-execution-056c
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Server Agent Routing Tool Execution Services"
metadata:
  actual:
    adr_id: 056c0240-a58c-4fa6-969e-8dcdffcf8d1b
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Server Agent Routing Tool Execution Services

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for server-side routes and services implementing autonomous agents, tool execution registries, or model evaluation harnesses, and modules integrating conversational agent workflows with external communication protocols or client adapters.

### Rules

- **R-AIE-001** MUST: The server agent routing and tool execution services MUST use the ai library as the standard runtime interface for model invocation, agent orchestration, and evaluation routines.

### Verify

```bash
# Discover the project script runner and run the test suite covering agent and evaluation routes.
# Discover and run the project typecheck script across server packages to verify compliance with the adopted library interfaces.
# Discover and run the repository linter to ensure agent tool definitions satisfy schema validation constraints.
```

**Accept when:**
- The project test suite passes with all agent orchestration and evaluation tests succeeding.
- The project static analysis and type verification checks complete with zero errors across agent route implementations.
- All tool definitions invoked by the agent runner conform to parsed input validation schemas without unvalidated parameter passthrough.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test pipelines, static analysis, type checking, and peer code review.
</enforcement>