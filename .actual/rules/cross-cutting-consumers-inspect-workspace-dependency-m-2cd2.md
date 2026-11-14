---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-m-2cd2
description: "Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Consumers Inspect Workspace Dependency Manifest Repository"
metadata:
  actual:
    adr_id: 2cd268a5-bb56-4674-9f41-676ee01130da
    status: accepted
    candidate_group_id: f8b4ae84-5a62-426c-ba59-a50291856ca2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Consumers Inspect Workspace Dependency Manifest Repository

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for status page proxy resolution handlers, access control predicate evaluators, and request rewrite logic.

### Rules

- **R-SCHEMA-001** MUST: Consumers MUST inspect the workspace dependency manifest and repository lock artifact to determine and verify the exact resolved version of shared schema packages before implementing proxy resolution logic.
- **R-SCHEMA-002** MUST: Execute lock-version grounding steps in order (find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation, confirm API existence, re-run for version-sensitive behavior) before writing code that uses a versioned library.
- **R-SCHEMA-003** SHOULD: Import schema models as types whenever runtime database query builder objects are not required in proxy resolution.
- **R-SCHEMA-004** SHOULD: Encapsulate schema evaluation logic inside dedicated predicate functions to maintain single-responsibility separation in routing handlers.

### Verify

```bash
# Discover and run the repository type-checking script to verify cross-package schema compatibility across proxy resolution handlers.
# Discover and execute the test runner for proxy routing resolution to validate access predicate behavior against schema models.
# Discover and execute the repository dependency boundary linter to confirm adherence to package import boundaries.
```

**Accept when:**
- Type checking passes without errors across all proxy resolution handlers importing @openstatus/db/src/schema.
- All proxy access control and rewrite unit tests pass successfully.
- No duplicate local interface definitions exist for entities provided by @openstatus/db/src/schema.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>