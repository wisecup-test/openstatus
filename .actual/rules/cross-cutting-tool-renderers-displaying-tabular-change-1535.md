---
version: 2
name: cross-cutting-tool-renderers-displaying-tabular-change-1535
description: "Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Tool Renderers Displaying Tabular Changes State"
metadata:
  actual:
    adr_id: 15359596-1490-4c06-9353-564a466193dc
    status: accepted
    candidate_group_id: 5734c25c-6eab-4e00-80c4-3e1fde6ed343
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Tool Renderers Displaying Tabular Changes State

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TOOL-001** SHOULD: Tool renderers displaying tabular changes or state diffs SHOULD reuse shared workspace table components to maintain presentation consistency across views.
- **R-TOOL-002** MANDATORY: Register each new tool renderer component in the central tool registry mapping corresponding to its contract in @openstatus/services/agent-tools.
- **R-TOOL-003** MANDATORY: Ensure fallback logging captures unmapped tool names in development and diagnostic environments to accelerate renderer implementation.
- **R-TOOL-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation for that exact version. (5) Confirm every API, class, or function exists in that exact version. (6) Re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover and run the workspace dependency verification command to ensure package resolution matches repository lock specifications.
# Discover and execute the project typecheck script to validate that all tool renderers conform to @openstatus/services/agent-tools contracts.
# Discover and run the test suite covering conversational tool rendering to confirm fallback handling for unmapped tool identifiers.
```

**Accept when:**
- All tool renderer components typecheck without errors against @openstatus/services/agent-tools contracts.
- The tool registry renders supported tools and triggers the expected fallback warning for unmapped tools.
- Workspace lockfile verification confirms all packages resolve to exact locked versions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking, pull request reviews, and automated test suites enforce renderer registration, type signatures, and fallback rendering behavior.
</enforcement>