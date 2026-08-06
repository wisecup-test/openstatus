# Standardize Query-Based Data Access with Authorization Headers for Primary Datastores: Services Implement Dedicated

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active and governs all service boundary interactions with primary datastores.

## Context

- Services interact with primary datastores through query-based access patterns that require authorization headers for authentication and access control.
- Multiple service boundaries (workflows cron service, checker service, private-location service, status page API router) independently implement query construction and header-based authorization when accessing external datastores.
- The codebase demonstrates a consistent pattern of constructing query parameters and setting authorization headers with bearer tokens retrieved from environment configuration.
- Service definitions expose endpoints that coordinate datastore queries with middleware-enforced authorization checks before allowing access to underlying data operations.

## Problem Statement

Services require a standardized approach to access primary datastores across service boundaries while maintaining consistent authorization, query construction, and error handling patterns. Without explicit guidance, teams may implement divergent access patterns that compromise security, observability, or maintainability.

## Decision

1. SHOULD: Services SHOULD implement dedicated client abstractions that encapsulate query construction, authorization header injection, and error handling for each primary datastore.

## Policy Block

- SHOULD Services SHOULD implement dedicated client abstractions that encapsulate query construction, authorization header injection, and error handling for each primary datastore.

In scope:
- All service boundary code that accesses primary datastores
- Client abstractions and wrappers for external datastore APIs
- Middleware layers that enforce authorization before datastore access
- API routers and endpoints that coordinate datastore queries

Out of scope:
- In-memory caching layers that do not directly query datastores
- Static configuration files that define datastore connection parameters
- Development and testing environments using mock datastore implementations

Exceptions:
- EXC-001: Internal administrative tools require direct datastore access for operational maintenance

## Rationale

- The evidence shows 4 distinct service boundaries implementing consistent query-based access patterns with authorization headers, indicating an established architectural convention.
- Standardizing this pattern reduces security risks by ensuring all datastore access is authenticated and authorized through a uniform mechanism.
- Query builder patterns and header-based authorization provide clear separation between service logic and datastore access concerns, improving testability and maintainability.
- The pattern's 91.20% confidence across multiple services demonstrates its stability and suitability as an architectural standard.

## Consequences

Positive:
- Consistent authorization and authentication patterns across all service boundaries reduce security vulnerabilities and audit complexity.
- Centralized client abstractions simplify testing by providing clear injection points for mock datastores.
- Query builder patterns enable static analysis and validation of datastore access patterns.
- Standardized error logging improves observability and incident response across services.

Negative:
- Additional abstraction layers may introduce latency for simple datastore queries.
- Teams must maintain client wrapper code for each primary datastore, increasing initial development effort.
- Schema validation overhead may impact performance for high-throughput query paths.
- Authorization middleware adds complexity to service initialization and configuration management.

## Alternatives

- Direct datastore SDK usage without client abstractions (rejected)
  Rejected because: Direct SDK usage scatters authorization logic across service code, making security audits difficult and increasing the risk of inconsistent credential handling.
  When valid: Acceptable only for prototype or proof-of-concept code that will not reach production.
- Service mesh or sidecar proxy for transparent authorization injection (deferred)
  Rejected because: Infrastructure-level authorization requires additional operational complexity and may not provide application-level query validation.
  When valid: Consider when migrating to a service mesh architecture or when cross-cutting authorization policies become too complex to manage in application code.
- Shared datastore access library across all services (accepted)
  When valid: When multiple services access the same primary datastore and can benefit from shared query patterns and authorization logic.

## Risks

- Credential leakage through logging or error messages that expose authorization tokens
  Mitigation: Implement credential scrubbing in logging infrastructure and ensure error messages never include raw authorization headers
  Owner: Security team and service owners
- Performance degradation from schema validation and middleware authorization checks on high-throughput query paths
  Mitigation: Profile critical paths and implement selective validation or caching for frequently accessed queries
  Owner: Engineering team
- Client abstraction drift where different services implement incompatible query patterns for the same datastore
  Mitigation: Establish shared client libraries and conduct regular architecture reviews to identify and consolidate divergent implementations
  Owner: Platform team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Examine existing client implementations in the codebase to identify common patterns for query construction, header injection, and error handling that should be extracted into shared abstractions.
- Ensure authorization middleware validates token format and expiration before allowing requests to proceed to datastore query logic.
- Implement integration tests that verify authorization failures return appropriate error codes and do not leak credential information in response bodies.

## Continuation Context


Verify commands:
- Discover the project's dependency resolution artifact and identify the exact versions of datastore client libraries in use across all services.
- Locate and execute the project's static analysis or linting configuration to verify that authorization headers are present in all datastore client instantiations.
- Identify and run the project's integration test suite that validates authorization middleware behavior and datastore access patterns.

Accept when:
- All datastore client code includes authorization header injection with credentials sourced from runtime configuration.
- Static analysis confirms no direct datastore SDK usage bypasses client abstractions or authorization middleware.
- Integration tests demonstrate that unauthorized requests to datastore-backed endpoints return 401 status codes without executing queries.

## Enforcement

- Verified by: Automated static analysis in continuous integration pipelines scanning for direct datastore SDK imports without authorization wrappers
- Verified by: Code review checklists requiring verification of authorization middleware and query validation for all new datastore access code
- Verified by: Integration test coverage requirements ensuring all datastore-backed endpoints have authorization failure test cases
- Violation handling: Pull requests introducing datastore access without proper authorization patterns are blocked from merging until corrected
- Violation handling: Security scanning tools flag credential handling violations as high-severity findings requiring immediate remediation
- Violation handling: Architecture review board escalates repeated violations for team-level training and process improvement
- Exception process: Submit exception request to platform team documenting the specific use case, alternative security controls, and time-bound justification
- Exception process: Security team reviews and approves exceptions with mandatory audit logging and periodic re-evaluation requirements
- Exception process: Approved exceptions are documented in service architecture decision logs with expiration dates and renewal criteria