---
version: 2
name: cross-cutting-evaluation-harnesses-not-bypass-library-ed24
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Evaluation Harnesses Not Bypass Library Execution"
metadata:
  actual:
    adr_id: ed245298-7a9c-490c-8e87-e3db89b43eff
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Evaluation Harnesses Not Bypass Library Execution

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for server-side routes and services implementing autonomous agents, tool execution registries, or model evaluation harnesses, and modules integrating conversational agent workflows with external communication protocols or client adapters.

### Rules

- **R-AILIB-001** MUST_NOT: Evaluation harnesses MUST NOT bypass the ai library execution pathways when testing tool selection, arguments, and decision rationale.

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
Claude Code MUST NOT skip or defer verification. Continuous integration test pipelines, automated static analysis, type checking, and peer code review enforce these rules, blocking pull requests that introduce direct bespoke model calls or bypass tool schema validation.
</enforcement>