---
version: 2
name: cross-cutting-modules-defining-domain-specific-data-1c66
description: "Internal Schema Module (./schema): Modules Defining Domain Specific Data Contracts"
metadata:
  actual:
    adr_id: 1c667ecb-a20a-4ae6-aab0-ad9b65d35c6e
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

# Internal Schema Module (./schema): Modules Defining Domain Specific Data Contracts

Scope: applies to `apps/web/src/content/utils/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for feature and utility modules that ingest external data, parse structured files, or define domain-specific network payloads, and application submodules requiring localized schema contracts and input validation routines.

### Rules

- **R-SCH-001** MUST: Modules defining domain-specific data contracts or performing payload ingestion MUST encapsulate their contract definitions and validation logic within a co-located ./schema internal module.
- **R-SCH-002** MUST: Execute input validation via safeParse at module entry points to guarantee runtime type safety before internal processing or service definition dispatch.
- **R-SCH-003** MUST: Expose both validation schema objects and inferred contract types from the ./schema module so consumers utilize a single consistent import source for compile-time and runtime contracts.
- **R-SCH-004** MUST: Handle validation failures from safeParse defensively at module entry points, logging structured errors or returning predictable error envelopes before invoking downstream operations.

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
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, linting checks, and mandatory peer code review.
</enforcement>