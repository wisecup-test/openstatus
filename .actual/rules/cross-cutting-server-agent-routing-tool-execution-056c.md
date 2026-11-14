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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AGT-001** MUST: The server agent routing and tool execution services MUST use the ai library as the standard runtime interface for model invocation, agent orchestration, and evaluation routines.
- **R-AGT-002** MANDATORY: Execute lock-version grounding in order (find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for exact version, confirm API existence, re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-AGT-003** MANDATORY: Define all agent tools with explicit schema definitions and validate input payloads defensively before dispatching actions.
- **R-AGT-004** MANDATORY: Externalize target model identifiers to environment configurations so model endpoints can be updated across environments without modifying agent orchestrator code.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test pipelines, automated static analysis, and peer code reviews.
</enforcement>