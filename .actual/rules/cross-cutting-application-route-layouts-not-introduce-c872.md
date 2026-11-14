---
version: 2
name: cross-cutting-application-route-layouts-not-introduce-c872
description: "Adoption of next Framework for Route Layouts and Navigation Architecture: Application Route Layouts Not Introduce Custom"
metadata:
  actual:
    adr_id: c87225b2-48fb-453b-941b-9f8e63fc45ef
    status: accepted
    candidate_group_id: 04208163-6b55-4975-9444-52819ae6503a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next Framework for Route Layouts and Navigation Architecture: Application Route Layouts Not Introduce Custom

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public web application route layouts and navigation wrappers within the status page application service.

### Rules

- **R-ROUT-001** MUST_NOT: Application route layouts MUST NOT introduce custom ad-hoc routing abstractions or third-party navigation engines that bypass the next framework routing model.

### Verify

```bash
# Discover the repository package manifest and execute the configured type checking and linting tasks
# to validate layout component interfaces and import paths, then run the test suite.
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- All route layout components import next and next/navigation in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>