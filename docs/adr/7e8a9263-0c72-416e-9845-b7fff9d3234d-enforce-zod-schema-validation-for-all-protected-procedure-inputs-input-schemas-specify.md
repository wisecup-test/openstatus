# Enforce Zod Schema Validation for All Protected Procedure Inputs: Input Schemas Specify

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all protected procedures that accept external input parameters.

## Context

- The codebase implements protected procedures using tRPC that expose API endpoints requiring workspace-scoped authorization and input validation
- External inputs from API consumers include monitor IDs, domain names, pagination parameters, date ranges, and configuration objects that must be validated before database queries execute
- The pattern emerged across 17 files with 91.21% confidence, indicating systematic adoption of schema-based input validation using the Zod library
- Database queries construct WHERE clauses and filter conditions using validated input parameters, requiring type safety to prevent injection attacks and data integrity violations
- The architecture separates input validation from business logic by declaring schemas at procedure definition boundaries before query execution

## Problem Statement

Protected procedures that accept external input without schema validation expose the system to type coercion vulnerabilities, injection attacks, and runtime errors from malformed data. Without declarative validation at procedure boundaries, business logic must defensively check every parameter, creating scattered validation logic and increasing the attack surface for workspace-scoped data access violations.

## Decision

1. MUST: Input schemas MUST specify explicit types for all parameters including strings, numbers, dates, enums, arrays, and objects with nested validation rules

## Policy Block

- MUST Input schemas MUST specify explicit types for all parameters including strings, numbers, dates, enums, arrays, and objects with nested validation rules

In scope:
- All tRPC protected procedures that accept input parameters
- Public API endpoints exposed through router definitions
- Procedures that construct database queries using input parameters
- Endpoints that perform workspace-scoped authorization checks

Out of scope:
- Internal service functions that receive pre-validated data structures
- Database schema definitions and ORM model declarations
- Response serialization and output formatting logic
- Middleware that operates on request context rather than input parameters

Exceptions:
- EXC-001: A procedure accepts no input parameters and operates solely on authenticated context
- EXC-002: Legacy procedures undergoing incremental migration to the validation pattern

## Rationale

- The evidence shows 17 files implementing consistent schema validation patterns with 91.21% confidence, indicating this is an established architectural standard rather than isolated usage
- Protected procedures construct SQL WHERE clauses using input parameters, requiring type-safe validation to prevent injection attacks and ensure workspace isolation boundaries are enforced correctly
- Declarative schemas at procedure boundaries provide self-documenting API contracts that enable automatic type generation for API consumers and reduce runtime validation errors
- The pattern separates input validation concerns from business logic, allowing database query construction to assume validated types and reducing defensive programming overhead

## Consequences

Positive:
- Input validation failures are caught at procedure boundaries before database queries execute, preventing invalid data from reaching persistence layers
- Schema declarations serve as machine-readable API contracts that enable automatic TypeScript type generation for frontend consumers
- Centralized validation logic at procedure definitions eliminates scattered parameter checks throughout business logic and query construction code
- Enum validation prevents invalid constant values from bypassing workspace authorization checks or triggering undefined database query behavior

Negative:
- Schema definitions increase the verbosity of procedure declarations, requiring maintenance when input requirements change
- Complex nested validation rules can become difficult to debug when validation errors do not clearly indicate which nested field failed
- Schema parsing adds runtime overhead to every procedure invocation, though this is typically negligible compared to database query latency
- Tight coupling to a specific validation library creates migration costs if the library is deprecated or replaced

## Alternatives

- Implement manual type checking and validation within each procedure using conditional statements and type guards (rejected)
  Rejected because: Manual validation scatters validation logic across business logic, increases code duplication, and provides no machine-readable contract for type generation
  When valid: Only acceptable for internal service functions that receive pre-validated data from trusted callers
- Rely on TypeScript compile-time types without runtime validation (rejected)
  Rejected because: TypeScript types are erased at runtime and provide no protection against malformed external input from API consumers or serialization boundaries
  When valid: Acceptable only for internal module boundaries where all callers are TypeScript code compiled together
- Implement validation using JSON Schema with a separate validation library (deferred)
  Rejected because: The evidence shows consistent adoption of the detected validation library across 17 files; migration would require coordinated refactoring without clear benefit
  When valid: Could be reconsidered if the project adopts a polyglot API gateway requiring language-agnostic schema definitions

## Risks

- Schema validation failures may expose internal implementation details through error messages that leak database structure or business logic constraints
  Mitigation: Implement error transformation middleware that sanitizes validation errors before returning them to API consumers, removing internal field names and constraint details
  Owner: API security team
- Complex schemas with deep nesting or recursive validation may introduce performance bottlenecks for high-throughput endpoints
  Mitigation: Profile validation overhead for critical paths and consider caching parsed schemas or implementing fast-path validation for common input patterns
  Owner: Performance engineering team
- Schema definitions may drift from actual business requirements if not updated when input requirements change, causing false validation failures
  Mitigation: Include schema validation tests in integration test suites that verify schemas accept all valid production input patterns
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
- Locate existing protected procedure definitions in router modules to identify the validation library import and schema declaration patterns used throughout the codebase
- For procedures that construct database WHERE clauses, ensure schema validation occurs before any query builder methods are invoked to prevent partially-validated parameters from reaching SQL construction
- When adding new enum-based parameters, define enum constants in a shared schema module and reference them in both validation schemas and database schema definitions to maintain consistency
- Test schema validation by writing unit tests that verify both valid inputs are accepted and invalid inputs are rejected with appropriate error messages

## Continuation Context


Verify commands:
- Discover the project's test execution script in the dependency manifest and run the test suite targeting router modules to verify schema validation coverage
- Locate the project's static analysis configuration and execute the type checker to confirm all procedure input parameters have corresponding schema declarations
- Search the codebase for protected procedure definitions and verify each procedure that accepts input parameters includes a schema validation declaration before business logic execution

Accept when:
- All protected procedures that accept input parameters include schema validation declarations at procedure boundaries
- Test suite includes validation tests that verify schemas reject invalid input types, missing required fields, and out-of-range enum values
- Static analysis confirms no database queries are constructed using unvalidated input parameters from procedure inputs

## Enforcement

- Verified by: Code review checklist requires reviewers to verify schema validation is present for all new protected procedures
- Verified by: Static analysis in continuous integration pipeline detects procedures that accept input parameters without schema declarations
- Verified by: Integration tests validate that invalid input to protected procedures returns validation errors rather than database errors
- Violation handling: Pull requests that introduce protected procedures without schema validation are blocked from merging until validation is added
- Violation handling: Existing procedures without validation are tracked in technical debt register with priority based on exposure to external input
- Violation handling: Security review escalates procedures that construct database queries from unvalidated input for immediate remediation
- Exception process: Engineer submits exception request documenting why schema validation is not applicable for the specific procedure
- Exception process: Technical lead reviews the request and confirms the procedure either accepts no input or receives only pre-validated internal data
- Exception process: Approved exceptions are documented in procedure comments with justification and reviewed quarterly for continued validity