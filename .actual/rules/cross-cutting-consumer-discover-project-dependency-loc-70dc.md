---
version: 2
name: cross-cutting-consumer-discover-project-dependency-loc-70dc
description: "Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Consumer Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 70dce7b7-3054-46ac-b862-b2f86be05790
    status: accepted
    candidate_group_id: 34a7a20c-e8d1-4a1c-a1bd-c5720a4ba1b1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/stripe/**
---

# Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Consumer Discover Project Dependency Lock Artifact

Scope: applies to `packages/api/src/router/stripe/**`.

These rules are ALWAYS ACTIVE for all backend modules, services, and route handlers that interact with external billing providers, process subscription updates, or map price identifiers to application plans.

### Rules

- **R-STRIPE-001** MUST: The consumer MUST discover the project dependency lock artifact and resolve the exact locked version of the Stripe library before developing or modifying billing integrations.
- **R-STRIPE-002** MANDATORY: Execute in order to lock-version ground: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run for version-sensitive behavior at point of use.
- **R-STRIPE-003** MANDATORY: Encapsulate all subscription mutation operations within dedicated billing router modules to prevent client instances from leaking into domain layers.
- **R-STRIPE-004** MANDATORY: Ensure environment-specific price lookup tables are validated against active payment gateway configuration during test execution.

### Verify

```bash
# Discover and execute the project verification test suite for billing and subscription router modules.
# Discover and execute the repository type-checking script across modules interfacing with external payment SDKs.
# Discover and execute the linting and static analysis suite to verify subscription lifecycle integration constraints.
```

**Accept when:**
- All subscription lifecycle operations execute through the designated library client methods without direct HTTP bypasses.
- Price lookup utilities accurately resolve environment-specific price identifiers to internal plan definitions.
- Project verification and type-checking suites pass without diagnostic errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated type checking, linting, and test suites verify subscription lifecycle integration constraints, and builds fail upon unapproved direct HTTP calls to payment endpoints or untyped price lookups.
</enforcement>