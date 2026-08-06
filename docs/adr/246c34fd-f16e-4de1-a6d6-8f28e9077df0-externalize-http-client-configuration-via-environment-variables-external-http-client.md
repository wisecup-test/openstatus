# Externalize HTTP Client Configuration via Environment Variables: External Http Client

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active and governs all external HTTP client configuration patterns.

## Context

- External HTTP clients require runtime configuration for endpoint URLs and authentication credentials that vary across deployment environments
- Hard-coding service URLs and API keys in source code creates security vulnerabilities and prevents environment-specific deployments
- The codebase uses environment variables to inject runtime configuration for external HTTP clients, separating deployment concerns from application logic
- HTTP clients construct request URLs dynamically using query parameters and set authorization headers with bearer tokens retrieved from the environment

## Problem Statement

External HTTP clients must connect to different service endpoints and authenticate with different credentials across development, staging, and production environments. Hard-coding these values prevents secure, environment-specific deployments and violates the principle of separating configuration from code.

## Decision

1. MUST: External HTTP client implementations MUST retrieve service endpoint URLs from environment variables at runtime rather than hard-coding them in source code

## Policy Block

- MUST External HTTP client implementations MUST retrieve service endpoint URLs from environment variables at runtime rather than hard-coding them in source code

In scope:
- All HTTP clients that communicate with external third-party services
- Client implementations that require authentication credentials
- Service endpoint URLs that vary across deployment environments
- Configuration values that contain sensitive data or secrets

Out of scope:
- Internal service-to-service communication within the same deployment boundary
- Static configuration values that are identical across all environments
- HTTP clients used exclusively in test environments with mock servers
- Configuration values that are public and non-sensitive

Exceptions:
- EXC-001: Local development environments may use default localhost URLs when environment variables are not set
- EXC-002: Integration tests may inject configuration programmatically rather than via environment variables

## Rationale

- Environment variable injection enables the same compiled binary to run in multiple environments without recompilation or code changes
- Separating credentials from source code prevents accidental exposure through version control and reduces the attack surface for credential theft
- The evidence shows explicit use of environment variable retrieval for URL configuration and authorization header construction with bearer tokens
- Dynamic query parameter construction and header setting patterns indicate a flexible, runtime-configurable HTTP client architecture

## Consequences

Positive:
- Deployment flexibility: the same application binary can be deployed to multiple environments with different configurations
- Security improvement: credentials are never committed to version control and can be rotated without code changes
- Operational simplicity: configuration changes do not require recompilation or redeployment of application code
- Testing isolation: integration tests can inject test-specific configurations without affecting production code

Negative:
- Runtime configuration errors are detected later in the application lifecycle compared to compile-time validation
- Environment variable management adds operational complexity and requires coordination across deployment tooling
- Missing or misconfigured environment variables can cause silent failures or runtime panics if not properly validated
- Debugging configuration issues requires access to the runtime environment rather than just the source code

## Alternatives

- Use configuration files with environment-specific variants committed to version control (rejected)
  Rejected because: Configuration files containing credentials would be committed to version control, creating security vulnerabilities and requiring separate secret management
  When valid: Valid for non-sensitive configuration values that benefit from version control and code review
- Use a centralized configuration service with runtime service discovery (rejected)
  Rejected because: Adds infrastructure complexity and external dependencies for simple HTTP client configuration that can be managed with environment variables
  When valid: Valid for large-scale distributed systems with hundreds of configuration parameters and complex dependency graphs
- Hard-code configuration values and use build-time substitution for environment-specific builds (rejected)
  Rejected because: Requires separate builds for each environment, complicates deployment pipelines, and prevents runtime configuration changes
  When valid: Valid for embedded systems or static deployments where runtime configuration is not feasible

## Risks

- Missing or misconfigured environment variables cause runtime failures that are difficult to diagnose in production
  Mitigation: Implement validation logic in client constructors that fails fast with clear error messages when required environment variables are missing or invalid
  Owner: Engineering team
- Environment variables containing credentials may be logged or exposed through error messages or debugging output
  Mitigation: Implement structured logging that redacts credential values and ensure error messages do not include full environment variable contents
  Owner: Security team
- Environment variable naming collisions across multiple HTTP clients create configuration conflicts
  Mitigation: Establish a consistent naming convention with service-specific prefixes and document all required environment variables in a central registry
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
- Client constructors should retrieve environment variables during initialization and store them as private fields, failing fast with descriptive errors if required variables are missing
- Use structured logging with context to record configuration retrieval and validation, ensuring credential values are redacted from log output
- Document all required environment variables in the client package documentation, including expected format, example values, and environment-specific variations

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
- Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
- Search the codebase for environment variable retrieval patterns and verify that credentials are not hard-coded or committed to version control

Accept when:
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration

## Enforcement

- Verified by: Code review process verifies that new HTTP clients follow environment variable configuration patterns
- Verified by: Static analysis tools scan for hard-coded URLs and credentials in HTTP client implementations
- Verified by: Integration tests in continuous integration pipelines validate environment variable configuration
- Violation handling: Code review blocks merge requests that hard-code credentials or service URLs
- Violation handling: Static analysis failures in CI pipelines prevent deployment of code with configuration violations
- Violation handling: Security scanning tools flag committed credentials and trigger incident response procedures
- Exception process: Request exception approval from engineering team lead with written justification
- Exception process: Document the exception in the client implementation with clear comments explaining the rationale
- Exception process: Schedule technical debt review to evaluate whether the exception can be removed in future iterations