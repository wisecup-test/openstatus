# Use Environment Variables for Runtime Configuration Secrets: Environment Variable Retrieval

Status: proposed
Date: 2025-01-17
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active and governs all runtime configuration and secrets handling across the system.

## Context

- The system requires runtime configuration for external service endpoints, authentication credentials, and API keys that vary across deployment environments
- Security-sensitive values including API keys, service account credentials, and authorization tokens must be injected at runtime without hardcoding in source code
- Multiple services require access to environment-specific configuration including database connections, external API endpoints, and cloud service credentials
- The codebase uses standard library environment variable access patterns with direct retrieval at service initialization time

## Problem Statement

Services need a consistent, secure mechanism to obtain runtime configuration and secrets that works across development, staging, and production environments without embedding sensitive values in source code or requiring complex configuration file management.

## Decision

1. MUST: Environment variable retrieval MUST occur during service initialization or client construction, not at request-processing time

## Policy Block

- MUST Environment variable retrieval MUST occur during service initialization or client construction, not at request-processing time

In scope:
- All service initialization code that requires runtime configuration
- HTTP client constructors that need API endpoints or authentication credentials
- Database connection initialization requiring connection strings or credentials
- External service integrations requiring API keys or service account credentials
- Cloud service clients requiring project identifiers or authentication material

Out of scope:
- Build-time configuration embedded in compiled binaries
- Development-only configuration used exclusively in local testing
- Public configuration values that do not vary by environment

Exceptions:
- EXC-001: Local development environments where developers use configuration files for convenience

## Rationale

- Evidence shows consistent use of environment variable retrieval for API endpoints, authentication credentials, and service configuration across multiple service boundaries
- The pattern separates configuration concerns from application logic, enabling the same binary to run in different environments without recompilation
- Security-sensitive values including private keys, API keys, and service credentials are consistently sourced from environment variables rather than configuration files or hardcoded values
- The approach aligns with twelve-factor application principles for configuration management and is compatible with container orchestration and cloud deployment platforms

## Consequences

Positive:
- Secrets and credentials never appear in source code or version control, reducing security exposure
- The same compiled binary can be deployed across all environments with different configuration injected at runtime
- Configuration changes do not require code changes or recompilation, enabling faster deployment cycles
- The pattern integrates naturally with container orchestration platforms and cloud secret management services

Negative:
- Environment variable management becomes a deployment concern requiring coordination between development and operations teams
- Missing or misconfigured environment variables may only be detected at runtime rather than at build time
- Debugging configuration issues requires access to the runtime environment rather than inspecting source code alone
- No compile-time type safety or validation for configuration values retrieved from environment variables

## Alternatives

- Use configuration files with environment-specific variants checked into version control (rejected)
  Rejected because: Configuration files containing secrets in version control create security risks and violate the principle of separating code from configuration
  When valid: Acceptable for non-sensitive configuration in development environments only
- Use a centralized configuration service with runtime API calls to fetch configuration (rejected)
  Rejected because: Adds complexity and external dependencies for service initialization, and the evidence shows direct environment variable access is the established pattern
  When valid: May be appropriate for dynamic configuration that changes without redeployment or for very large-scale systems
- Use command-line flags for runtime configuration (rejected)
  Rejected because: Command-line arguments are visible in process listings and logs, creating security exposure for sensitive values
  When valid: Acceptable for non-sensitive operational flags like verbosity levels or feature toggles

## Risks

- Missing environment variables cause runtime failures that may not be detected until deployment
  Mitigation: Implement initialization-time validation that checks for required environment variables and fails fast with clear error messages. Add deployment verification steps that confirm required variables are set.
  Owner: Engineering team and DevOps team
- Environment variables may be logged or exposed in error messages, leaking sensitive values
  Mitigation: Implement logging and error handling that redacts environment variable values. Use structured logging with explicit field control rather than dumping entire environment maps.
  Owner: Engineering team
- Inconsistent environment variable naming across services creates operational complexity
  Mitigation: Establish and document a naming convention for environment variables. Create a central registry of required variables per service. Use automated tooling to validate variable names against the convention.
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
- Retrieve environment variables during service initialization or client constructor execution, storing values in struct fields or configuration objects rather than accessing environment variables repeatedly during request processing
- Implement validation logic that checks for required environment variables at startup and returns descriptive errors identifying which variables are missing, preventing silent failures or cryptic runtime errors
- Document all required environment variables for each service including their purpose, expected format, and whether they contain sensitive values, maintaining this documentation alongside deployment configuration

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the configured linter to detect hardcoded credentials or configuration values in source files
- Locate the project's test suite and execute integration tests that verify services fail gracefully when required environment variables are missing
- Identify the project's code search tooling and scan for environment variable retrieval patterns to confirm they occur during initialization rather than request processing

Accept when:
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline scanning for hardcoded secrets and credentials
- Verified by: Code review checklist requiring verification that new configuration uses environment variables
- Verified by: Integration test suite validating environment variable handling and missing-variable error cases
- Violation handling: Static analysis failures block pull request merging until hardcoded values are removed
- Violation handling: Code review identifies violations and requires changes before approval
- Violation handling: Security scanning tools flag exposed credentials for immediate remediation
- Exception process: Request exception through team lead with documented justification for why environment variables cannot be used
- Exception process: Security team review required for any exception involving sensitive values
- Exception process: Approved exceptions must be documented in code comments and tracked in the exception registry