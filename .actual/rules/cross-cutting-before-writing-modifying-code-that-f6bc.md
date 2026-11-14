---
version: 2
name: cross-cutting-before-writing-modifying-code-that-f6bc
description: "Adoption of next Framework for Route Layouts and Navigation Architecture: Before Writing Modifying Code That Uses"
metadata:
  actual:
    adr_id: f6bc0dcb-a6fb-4eca-9ebb-37df497f853e
    status: accepted
    candidate_group_id: 04208163-6b55-4975-9444-52819ae6503a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next Framework for Route Layouts and Navigation Architecture: Before Writing Modifying Code That Uses

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public web application route layouts and navigation wrappers within the status page application service, and all domain- and locale-parameterized route segment hierarchies requiring layout composition and alternate metadata.

### Rules

- **R-ADR-001** MUST: Before writing or modifying code that uses framework libraries, the consumer MUST discover the ecosystem lock artifact and confirm that all referenced APIs exist within the exact resolved version documentation.

### Verify

```bash
# Discover the repository package manifest and execute the configured type checking and linting tasks
# to validate layout component interfaces and import paths, then run the test suite.
# (Exact commands derived from project repository manifest/package.json)
```

**Accept when:**
- All route layout components import required framework modules in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>