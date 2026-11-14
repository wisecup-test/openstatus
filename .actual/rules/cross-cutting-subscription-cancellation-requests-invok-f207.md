---
version: 2
name: cross-cutting-subscription-cancellation-requests-invok-f207
description: "Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Subscription Cancellation Requests Invoke State Transitions"
metadata:
  actual:
    adr_id: f207ce00-8968-43c4-b6c4-81e627063c8c
    status: accepted
    candidate_group_id: 34a7a20c-e8d1-4a1c-a1bd-c5720a4ba1b1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/stripe/**
---

# Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Subscription Cancellation Requests Invoke State Transitions

Scope: applies to `packages/api/src/router/stripe/**`.

These rules are ALWAYS ACTIVE for all backend modules, services, and route handlers that interact with external billing providers, process subscription updates, or map price identifiers to application plans.

### Rules

- **R-STRIPE-001** MUST: Subscription cancellation requests MUST invoke subscription state transitions via the Stripe library subscriptions update API with explicit cancellation parameters and period-end scheduling rather than immediate unrecorded termination.

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
Claude Code MUST NOT skip or defer verification. All billing router modifications and subscription lifecycle updates must comply with these rules and pass verification checks.
</enforcement>