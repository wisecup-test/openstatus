---
version: 2
name: cross-cutting-application-use-next-primary-web-1aa8
description: "Adoption of next Framework for Route Layouts and Navigation Architecture: Application Use Next Primary Web Framework"
metadata:
  actual:
    adr_id: 1aa8d8e8-71c1-4910-80ac-17fe6ef3d9ad
    status: accepted
    candidate_group_id: 04208163-6b55-4975-9444-52819ae6503a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next Framework for Route Layouts and Navigation Architecture: Application Use Next Primary Web Framework

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public web application route layouts, navigation wrappers, and domain- and locale-parameterized route segment hierarchies within the application service.

### Rules

- **R-NEXT-001** MUST: The application MUST use next as the primary web application framework for routing, hierarchical layout structuring, and navigation across all public application route segments.

### Verify

```bash
# Discover the repository package manifest and execute type checking and linting tasks
# (Tool names and commands must be discovered from the repository manifest)
```

**Accept when:**
- All route layout components import next and next/navigation in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type checking, and peer code reviews enforce compliance.
</enforcement>