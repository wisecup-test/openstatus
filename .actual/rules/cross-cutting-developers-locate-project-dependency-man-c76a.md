---
version: 2
name: cross-cutting-developers-locate-project-dependency-man-c76a
description: "Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Developers Locate Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: c76ae2ea-1358-449b-9104-b18e29756987
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

# Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Developers Locate Project Dependency Manifest Lock

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all network probing services, edge diagnostic modules, presentation components, and data aggregation summaries that evaluate HTTP response headers or display CDN/cache evaluation results.

### Rules

- **R-HDR-001** MUST: Developers MUST locate the project dependency manifest and lock resolution artifact to verify the exact resolved version of all shared modules prior to introducing code changes.
- **R-HDR-002** MUST: Consume shared header analysis routines rather than manipulating raw response header maps directly within presentation or probing modules.
- **R-HDR-003** MUST: Couple header analysis outputs with schema validation schemas to enforce runtime data contracts.

### Verify

```bash
# Discover and execute the repository dependency audit and type checking verification scripts.
# Run the automated test suite across packages consuming the header analysis module to ensure contract compliance.
```

**Accept when:**
- All modules performing HTTP header inspection or CDN edge detection import and execute logic strictly through the designated header analysis module.
- Static type checks and automated project verification suites validate that response headers and cache metadata conform to the standardized schema contracts without ad-hoc header parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. All network diagnostic layers and presentation components must adhere strictly to the shared header analysis module and dependency lock verification policies.
</enforcement>