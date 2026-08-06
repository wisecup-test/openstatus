# Use console.error and structured logging for error tracking in query procedures: Query Procedures That

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all error handling in query procedures and API routes that interact with primary datastores.

## Context

- The codebase implements query procedures that interact with primary datastores through input validation, query execution, and error handling flows
- Error conditions arise from domain validation failures, database query failures, external service integration failures, and data inconsistency scenarios
- The system uses structured error logging with contextual metadata to capture domain-specific error details including entity identifiers, operation context, and failure reasons
- Multiple service boundaries exist including public procedures, protected procedures, and cron-triggered workflows that require consistent error visibility
- Error handling patterns coordinate with external error tracking services and middleware authorization checks

## Problem Statement

Query procedures that interact with primary datastores require consistent error logging to diagnose failures across validation, query execution, and external service integration boundaries. Without structured error logging that captures contextual metadata, debugging data inconsistencies, authorization failures, and query errors becomes difficult across distributed service boundaries.

## Decision

1. MUST: All query procedures that interact with primary datastores MUST log errors using structured logging that includes contextual metadata such as entity identifiers, operation type, and failure reason

## Policy Block

- MUST All query procedures that interact with primary datastores MUST log errors using structured logging that includes contextual metadata such as entity identifiers, operation type, and failure reason

In scope:
- All query procedures that execute database queries against primary datastores
- Public and protected API procedures that handle user input validation
- Service boundary layers that transform domain errors to API errors
- Cron workflows that execute periodic database operations
- External service integration points that may fail during query execution

Out of scope:
- Pure validation functions that do not interact with datastores
- Middleware layers that only perform authorization checks without query execution
- Client-side error handling and display logic
- Test fixtures and mock implementations

Exceptions:
- EXC-001: Query procedures in read-only reporting contexts where errors are expected and handled by retry logic

## Rationale

- The evidence shows 6 files implementing structured error logging with contextual metadata at query procedure boundaries, indicating a consistent pattern for error visibility
- Data inconsistency scenarios require warning-level logging that captures entity context without blocking operations, as demonstrated in the incident router filtering orphaned records
- Service boundary layers consistently log errors before transforming them for API consumers, enabling diagnosis of failures at the domain layer rather than only at the API surface
- Integration with external error tracking services and middleware authorization checks requires error logging to occur before error propagation to maintain complete error context

## Consequences

Positive:
- Improved error diagnosis across distributed service boundaries through structured contextual metadata
- Data inconsistency detection without operation blocking enables graceful degradation while maintaining visibility
- Consistent error logging patterns across public, protected, and cron-triggered workflows reduce cognitive load for debugging
- Integration with external error tracking services provides centralized error aggregation and alerting

Negative:
- Additional logging overhead in query execution paths may impact performance in high-throughput scenarios
- Structured logging requires coordination between domain error types and logging schema evolution
- Error logs may contain sensitive entity identifiers requiring log sanitization policies
- Inconsistent error logging patterns across legacy code require gradual migration effort

## Alternatives

- Use only external error tracking service integration without local structured logging (rejected)
  Rejected because: External service integration may fail or be unavailable, losing error visibility entirely. Local structured logging provides a fallback and enables local debugging without external service dependencies.
  When valid: In environments where external error tracking service availability is guaranteed and local log retention is not required
- Log errors only at the API boundary layer without domain-layer logging (rejected)
  Rejected because: API boundary logging loses domain context during error transformation, making it difficult to diagnose root causes in query execution and data inconsistency scenarios.
  When valid: In simple CRUD applications where domain logic is minimal and error context is fully preserved through transformation layers
- Use unstructured string logging without contextual metadata fields (rejected)
  Rejected because: Unstructured logging makes automated error aggregation, filtering, and alerting difficult. Structured metadata enables querying logs by entity identifier, operation type, and failure reason.
  When valid: In prototype or development environments where log analysis tooling is not available

## Risks

- Sensitive entity identifiers or user data may be logged in error contexts, creating compliance or privacy concerns
  Mitigation: Implement log sanitization policies that redact sensitive fields before logging. Review error logging patterns during security audits to ensure compliance with data protection requirements.
  Owner: Engineering team with security review
- Excessive error logging in high-throughput query paths may degrade performance or overwhelm log storage
  Mitigation: Implement sampling or rate-limiting for error logs in high-frequency scenarios. Monitor log volume and adjust logging levels based on operational needs.
  Owner: Engineering team with operations support
- Inconsistent error logging patterns across legacy and new code create gaps in error visibility
  Mitigation: Establish error logging standards in code review guidelines. Gradually migrate legacy code to structured logging patterns during maintenance cycles.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implement error logging immediately after catching exceptions in query procedures, before any error transformation or propagation to external services
- Structure error log metadata to include entity type, entity identifier, operation name, and failure reason as separate fields rather than concatenated strings
- Use warning-level severity for data inconsistency errors that are filtered rather than blocking operations, and error-level severity for query execution failures
- Coordinate error logging schema with external error tracking service integration to ensure consistent field naming and metadata structure

## Continuation Context


Verify commands:
- Discover the project's test execution mechanism and run integration tests that verify error logging occurs for query procedure failures
- Discover the project's static analysis tooling and verify that query procedures include error logging before error propagation
- Discover the project's log aggregation configuration and verify that structured error metadata fields are correctly indexed

Accept when:
- All query procedures that interact with primary datastores include structured error logging with contextual metadata
- Error logs include entity identifiers, operation type, and failure reason as separate structured fields
- Data inconsistency errors are logged at warning level without blocking operations
- Integration tests verify error logging occurs before error transformation or external service propagation

## Enforcement

- Verified by: Code review verification that query procedures include structured error logging before error propagation
- Verified by: Integration test coverage that asserts error logging occurs for query failure scenarios
- Verified by: Static analysis checks that verify error logging patterns at service boundary layers
- Violation handling: Code review rejection for query procedures that lack structured error logging
- Violation handling: Integration test failures for missing error logging in query failure paths
- Violation handling: Post-incident review when production errors lack sufficient context for diagnosis
- Exception process: Document exception rationale in code comments with engineering lead approval
- Exception process: Review exceptions during security audits to ensure no sensitive data exposure
- Exception process: Revisit exceptions during maintenance cycles to align with current logging standards