---
version: 2
name: cross-cutting-engineers-implementing-updating-agent-wo-bf0e
description: "Adoption of ai Library for Server Agent Orchestration and Evaluation: Engineers Implementing Updating Agent Workflows Inspect"
metadata:
  actual:
    adr_id: bf0e73b0-b2ca-485b-afd4-05450b145b15
    status: accepted
    candidate_group_id: 06200e5a-6e97-4e66-a12c-86635c94a18e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of ai Library for Server Agent Orchestration and Evaluation: Engineers Implementing Updating Agent Workflows Inspect

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Engineers implementing or updating agent workflows MUST inspect the repository dependency manifest and resolution artifact to verify the exact resolved version of the ai library prior to invoking its APIs.

### Verify

\`\`\`bash
# Discover the project script runner and run the test suite covering agent and evaluation routes.
# Discover and run the project typecheck script across server packages to verify compliance with the adopted library interfaces.
# Discover and run the repository linter to ensure agent tool definitions satisfy schema validation constraints.
\`\`\`

**Accept when:**
- The project test suite passes with all agent orchestration and evaluation tests succeeding.
- The project static analysis and type verification checks complete with zero errors across agent route implementations.
- All tool definitions invoked by the agent runner conform to parsed input validation schemas without unvalidated parameter passthrough.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified by continuous integration pipelines, automated static analysis, and peer code review.
</enforcement>