---
version: 2
name: cross-cutting-consumers-header-analysis-results-valida-606e
description: "Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Consumers Header Analysis Results Validate Evaluated"
metadata:
  actual:
    adr_id: 606e6490-8a54-47c5-99a8-50965a579d4c
    status: accepted
    candidate_group_id: 4dc8ceff-793e-4bd6-86a1-3c4c1bee59c4
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/cdn-checker/**
---

# Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Consumers Header Analysis Results Validate Evaluated

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** SHOULD: Consumers of header analysis results SHOULD validate evaluated attributes against shared schema contracts before propagating data to presentation layers or persistent stores.
- **R-HDR-002** MANDATORY: The consumer MUST discover dependency management tools, manifests, and lock files from the project repository.
- **R-HDR-003** MANDATORY: Before writing code that uses a versioned library, execute lock-version grounding to inspect exact resolved versions, consult official documentation, and confirm API existence.
- **R-HDR-004** MANDATORY: Consume shared header analysis routines rather than manipulating raw response header maps directly within presentation or probing modules.
- **R-HDR-005** MANDATORY: Couple header analysis outputs with schema validation schemas to enforce runtime data contracts.

### Verify

```bash
# Discover and execute the repository dependency audit and type checking verification scripts.
# Run the automated test suite across packages consuming the header analysis module to ensure contract compliance.
```

**Accept when:**
- All modules performing HTTP header inspection or CDN edge detection import and execute logic strictly through the designated header analysis module.
- Static type checks and automated project verification suites validate that response headers and cache metadata conform to the standardized schema contracts without ad-hoc header parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency and import boundary verification in continuous integration workflows and peer architecture reviews enforce these requirements, with build rejections applied to direct or duplicated header parsing implementations outside the canonical library.
</enforcement>