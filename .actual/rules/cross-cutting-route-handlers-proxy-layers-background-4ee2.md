---
version: 2
name: cross-cutting-route-handlers-proxy-layers-background-4ee2
description: "Internal Authentication Module (@/lib/auth) Adoption: Route Handlers Proxy Layers Background Utilities"
metadata:
  actual:
    adr_id: 4ee24672-d0f8-4ba7-9856-06bf5130e475
    status: accepted
    candidate_group_id: 0fa3c1d8-a1c4-4a91-8df9-e2bd52d4825b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Internal Authentication Module (@/lib/auth) Adoption: Route Handlers Proxy Layers Background Utilities

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, server actions, background task contexts, and edge proxy utilities requiring identity resolution or session access.

### Rules

- **R-AUTH-001** MUST_NOT: Route handlers, proxy layers, and background utilities MUST NOT bypass the centralized authentication module to invoke third-party authentication client libraries or inspect raw authentication cookies directly.

### Verify

```bash
# Discover the workspace static analysis script from repository configuration and execute it across all route and service boundaries.
# Locate the repository test runner from project configuration and run the test suite covering authentication utilities and dependent route handlers.
# Query repository imports to verify all session and identity operations originate from the designated internal authentication module.
```

**Accept when:**
- All static analysis and linting checks pass without violations of module boundary constraints.
- The automated test suite passes completely, verifying session resolution and authentication flows across dependent modules.
- Zero occurrences of direct third-party authentication provider imports exist outside the designated authentication library boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>