---
version: 2
name: cross-cutting-agent-definitions-resolve-model-identifi-42ac
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Definitions Resolve Model Identifiers Through"
metadata:
  actual:
    adr_id: 42acf5e0-e40a-4744-8348-1ec5a3892867
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Agent Definitions Resolve Model Identifiers Through

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for server-side routes and services implementing autonomous agents, tool execution registries, or model evaluation harnesses, as well as modules integrating conversational agent workflows with external communication protocols or client adapters.

### Rules

- **R-AI-001** MUST: Agent definitions MUST resolve model identifiers through runtime environment configuration rather than hardcoding static model names.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test pipelines, automated static analysis, and peer code review.
</enforcement>