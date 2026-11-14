---
version: 2
name: cross-cutting-inbound-data-payloads-ingested-feature-291f
description: "Internal Schema Module (./schema): Inbound Data Payloads Ingested Feature Handlers"
metadata:
  actual:
    adr_id: 291f2716-f659-4bbe-a707-0dd21a868641
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

# Internal Schema Module (./schema): Inbound Data Payloads Ingested Feature Handlers

Scope: applies to `apps/web/src/content/utils/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SCHM-001** MUST: Inbound data payloads ingested by feature handlers MUST be validated using safeParse prior to state transformation, persistence, or network transmission.
- **R-SCHM-002** MUST: Domain contract definitions and input validation schemas MUST reside in co-located ./schema modules adjacent to consuming logic.
- **R-SCHM-003** MUST: External payload ingestion points MUST invoke safeParse and handle validation errors prior to processing.

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