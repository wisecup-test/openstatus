---
version: 2
name: cross-cutting-agent-tool-execution-registries-validate-c306
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Tool Execution Registries Validate Input"
metadata:
  actual:
    adr_id: c30615b0-c420-47fc-806b-01ce78bb193d
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Tool Execution Registries Validate Input

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AI-001** MUST: Agent tool execution registries MUST validate input arguments against defined schemas before executing tool actions orchestrated through the ai library.

### Verify

```bash
# Discover and run the project script runner for tests covering agent and evaluation routes
# Discover and run the project typecheck script across server packages
# Discover and run the repository linter
```

**Accept when:**
- The project test suite passes with all agent orchestration and evaluation tests succeeding.
- The project static analysis and type verification checks complete with zero errors across agent route implementations.
- All tool definitions invoked by the agent runner conform to parsed input validation schemas without unvalidated parameter passthrough.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration test pipelines, automated static analysis, and peer code review.
</enforcement>