---
version: 2
name: cross-cutting-before-integrating-modifying-code-depend-c42f
description: "Adoption of @openstatus/utils Core Utility Library: Before Integrating Modifying Code Depending Versioned"
metadata:
  actual:
    adr_id: c42f51c5-82a4-40ad-aa0c-4506155df131
    status: accepted
    candidate_group_id: 24f82739-f335-4572-9c13-bfa2a59d6c93
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
      - src/page-subscriber/**
---

# Adoption of @openstatus/utils Core Utility Library: Before Integrating Modifying Code Depending Versioned

Scope: applies to `packages/services/src/monitor/**`, `packages/services/src/page-subscriber/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: Before integrating or modifying code depending on versioned modules, developers MUST inspect the repository dependency manifest and authoritative resolution artifact to verify exact dependency alignment.
- **R-UTIL-002** MUST: Follow the LOCK-VERSION GROUNDING process in order: find manifest, identify build tool, inspect lock/resolution artifact, look up official docs for that exact version, confirm every API/class/function exists before use, and re-run per dependency at point of use for version-sensitive behavior.
- **R-UTIL-003** MUST: Ensure inputs and outputs of shared utility library functions use generic primitives rather than domain-specific entity models.
- **R-UTIL-004** SHOULD: Organize utility functions into cohesion-focused submodules to allow tree-shaking and avoid unnecessary transitive imports in consuming services.

### Verify

```bash
# Discover and execute repository static analysis and linting scripts to verify internal module import boundaries
# Discover and run project test suites covering both shared utility modules and dependent service handlers
```

**Accept when:**
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>