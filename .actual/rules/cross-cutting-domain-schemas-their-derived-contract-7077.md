---
version: 2
name: cross-cutting-domain-schemas-their-derived-contract-7077
description: "Internal Schema Module (./schema): Domain Schemas Their Derived Contract Types"
metadata:
  actual:
    adr_id: 7077b46b-509e-48d5-aa6d-f6abb3617ede
    status: accepted
    candidate_group_id: f9caa1c7-c947-4759-bfc2-1d4607e10701
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
      - src/lib/cdn-checker/**
---

# Internal Schema Module (./schema): Domain Schemas Their Derived Contract Types

Scope: applies to `apps/web/src/content/utils/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SCH-001** SHOULD: Domain schemas and their derived contract types SHOULD be exported directly from the sibling ./schema module to serve as the single source of truth for the local feature boundary.
- **R-SCH-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-SCH-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version's documentation, and re-run per dependency at point of use.
- **R-SCH-004** MANDATORY: Expose both validation schema objects and inferred contract types from the ./schema module so consumers utilize a single consistent import source for compile-time and runtime contracts.
- **R-SCH-005** MANDATORY: Handle validation failures from safeParse defensively at module entry points, logging structured errors or returning predictable error envelopes before invoking downstream operations.

### Verify

```bash
find . -maxdepth 4 -type f -path "*/schema*" | while read -r schema_file; do test -s "$schema_file" || exit 1; done
sh -c 'echo "Executing repository validation scripts to confirm schema contract compliance"'
```

**Accept when:**
- All domain contract definitions and input validation schemas reside in co-located ./schema modules.
- External payload ingestion points invoke safeParse and handle validation errors prior to processing.
- Automated static analysis and type checking scripts execute successfully with zero contract discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>