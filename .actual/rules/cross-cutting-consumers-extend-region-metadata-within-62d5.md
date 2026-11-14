---
version: 2
name: cross-cutting-consumers-extend-region-metadata-within-62d5
description: "Adoption of @openstatus/regions for Canonical Region Definitions: Consumers Extend Region Metadata Within Downstream"
metadata:
  actual:
    adr_id: 62d540c5-2267-4945-9ecf-557c015ac2f9
    status: accepted
    candidate_group_id: e93ba485-cd34-462f-82e0-d234a52187de
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/regions for Canonical Region Definitions: Consumers Extend Region Metadata Within Downstream

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for route schemas, validation layers, RPC handlers, and protocol converters dealing with region identifiers and metadata.

### Rules

- **R-REG-001** MAY: Consumers MAY extend region metadata within downstream route schemas provided the underlying identity derives directly from @openstatus/regions definitions.
- **R-REG-002** MANDATORY: The consumer MUST derive build tools, file names, commands, package managers, and version numbers from the project repository.
- **R-REG-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run for version-sensitive behavior at point of use.
- **R-REG-004** MANDATORY: Locate the shared region module through repository workspace manifests and import canonical region types directly into route schemas and handler converters.
- **R-REG-005** MANDATORY: When defining input validation schemas or mapping RPC structures, consume the exported region structures directly rather than re-declaring literal unions.

### Verify

```bash
# Discover workspace manifest and execute static type-checking and analysis tasks
# Discover and run test suite to ensure schema validation and RPC converter mappings pass all contract tests
```

**Accept when:**
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests containing locally declared region enums or duplicate string literals will fail review and must be refactored to use the canonical module.
</enforcement>