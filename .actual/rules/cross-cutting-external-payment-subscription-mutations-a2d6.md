---
version: 2
name: cross-cutting-external-payment-subscription-mutations-a2d6
description: "Stripe SDK Adoption for Subscription Lifecycle and Billing Management: External Payment Subscription Mutations Billing Lifecycle"
metadata:
  actual:
    adr_id: a2d60e41-3d43-4895-8c3f-61ce4cf69aee
    status: accepted
    candidate_group_id: 34a7a20c-e8d1-4a1c-a1bd-c5720a4ba1b1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/stripe/**
---

# Stripe SDK Adoption for Subscription Lifecycle and Billing Management: External Payment Subscription Mutations Billing Lifecycle

Scope: applies to `packages/api/src/router/stripe/**`.

These rules are ALWAYS ACTIVE for all backend modules, services, and route handlers that interact with external billing providers, process subscription updates, or map price identifiers to application plans.

### Rules

- **R-STRIPE-001** MUST: All external payment, subscription mutations, and billing lifecycle operations MUST use the adopted Stripe library client rather than direct ad-hoc HTTP integrations.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>