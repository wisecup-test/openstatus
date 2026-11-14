---
version: 2
name: cross-cutting-agent-execution-services-record-telemetr-8481
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Execution Services Record Telemetry Tool"
metadata:
  actual:
    adr_id: 84811a8d-3d8c-4f0b-a200-d3a54f45802f
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Execution Services Record Telemetry Tool

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TELEMETRY-001** SHOULD: Agent execution services record execution telemetry for tool selection and argument resolution during evaluation and diagnostic runs.

### Verify

```bash
# Discover and run the project test suite covering agent and evaluation routes
# Discover and run the project typecheck script across server packages
# Discover and run the repository linter to ensure agent tool definitions satisfy schema validation constraints
```

**Accept when:**
- The project test suite passes with all agent orchestration and evaluation tests succeeding.
- The project static analysis and type verification checks complete with zero errors across agent route implementations.
- All tool definitions invoked by the agent runner conform to parsed input validation schemas without unvalidated parameter passthrough.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, automated static analysis, and peer code reviews enforce compliance.
</enforcement>