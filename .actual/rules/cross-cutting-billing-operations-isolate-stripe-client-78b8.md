---
version: 2
name: cross-cutting-billing-operations-isolate-stripe-client-78b8
description: "Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Billing Operations Isolate Stripe Client Initialization"
metadata:
  actual:
    adr_id: 78b8e1dd-3d90-4678-80cd-8e34478f197d
    status: accepted
    candidate_group_id: 34a7a20c-e8d1-4a1c-a1bd-c5720a4ba1b1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/stripe/**
---

# Stripe SDK Adoption for Subscription Lifecycle and Billing Management: Billing Operations Isolate Stripe Client Initialization

Scope: applies to `packages/api/src/router/stripe/**`.

These rules are ALWAYS ACTIVE for all backend modules, services, and route handlers that interact with external billing providers, process subscription updates, or map price identifiers to application plans.

### Rules

- **R-BILL-001** SHOULD: Billing operations SHOULD isolate Stripe client initialization and external price resolution utilities to dedicated payment router modules to maintain a clear boundary between billing infrastructure and business logic.
- **R-BILL-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-BILL-003** MANDATORY: Before writing code that uses a versioned library, execute lock-version grounding in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-BILL-004** MANDATORY: Encapsulate all subscription mutation operations within dedicated billing router modules to prevent client instances from leaking into domain layers.
- **R-BILL-005** MANDATORY: Ensure environment-specific price lookup tables are validated against active payment gateway configuration during test execution.

### Verify

```bash
# Discover and execute the project verification test suite for billing and subscription router modules
# Discover and execute the repository type-checking script across modules interfacing with external payment SDKs
# Discover and execute the linting and static analysis suite to verify subscription lifecycle integration constraints
```

**Accept when:**
- All subscription lifecycle operations execute through the designated library client methods without direct HTTP bypasses.
- Price lookup utilities accurately resolve environment-specific price identifiers to internal plan definitions.
- Project verification and type-checking suites pass without diagnostic errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon unapproved direct HTTP calls to payment endpoints or untyped price lookups.
</enforcement>