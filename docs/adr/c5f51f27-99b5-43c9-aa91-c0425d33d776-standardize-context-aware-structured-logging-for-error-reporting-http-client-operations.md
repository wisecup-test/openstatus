# Standardize Context-Aware Structured Logging for Error Reporting: Http Client Operations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all services that perform error logging in runtime operations.

## Context

- Services require consistent error reporting mechanisms that preserve request context across distributed operations and external integrations
- Environment-specific configuration values must be retrieved at runtime to support deployment across multiple environments without code changes
- External service integrations require authenticated HTTP clients with proper error handling and observability hooks
- Structured logging with context propagation enables correlation of errors across service boundaries and facilitates debugging in production environments

## Problem Statement

Services must report errors with sufficient context for debugging while maintaining consistent logging patterns across the codebase. Without standardized context-aware logging, error messages lack correlation identifiers, making it difficult to trace failures across service boundaries and diagnose issues in production environments.

## Decision

1. SHOULD: HTTP client operations that interact with external services SHOULD log errors with response status codes and request identifiers

## Policy Block

- SHOULD HTTP client operations that interact with external services SHOULD log errors with response status codes and request identifiers

In scope:
- All services that perform HTTP client operations to external APIs
- All error handling code paths in service handlers and background workers
- All operations that retrieve configuration from environment variables
- All logging statements that report operational errors or integration failures

Out of scope:
- Debug-level logging for development environments
- Performance metrics and telemetry data collection
- Application startup and shutdown lifecycle logging
- Third-party library internal logging behavior

Exceptions:
- EXC-001: Legacy code paths that are scheduled for deprecation within the current quarter
- EXC-002: Prototype or experimental features not yet promoted to production

## Rationale

- Evidence shows consistent use of log.Ctx(ctx).Error() pattern across multiple service files, indicating an established architectural pattern for context-aware error logging
- Multiple environment variable retrievals for API keys, credentials, and service URLs demonstrate a runtime configuration strategy that supports environment-specific deployments
- The presence of HTTP client operations with authorization headers and request body handling indicates external service integration patterns that require robust error reporting
- Structured logging with context propagation enables correlation of errors across distributed operations and supports production debugging workflows

## Consequences

Positive:
- Consistent error logging patterns improve debuggability and reduce mean time to resolution for production incidents
- Context propagation enables distributed tracing and correlation of errors across service boundaries
- Environment-based configuration supports deployment flexibility without code changes or recompilation
- Structured log entries facilitate automated log analysis, alerting, and anomaly detection

Negative:
- Context propagation requires discipline to pass context objects through all function call chains, increasing code verbosity
- Environment variable dependencies create implicit configuration contracts that may not be validated until runtime
- Structured logging libraries may introduce performance overhead compared to simple string formatting
- Sensitive data in context or environment variables requires careful handling to prevent accidental logging of secrets

## Alternatives

- Use global logger instances without context propagation (rejected)
  Rejected because: Global loggers cannot preserve request-scoped correlation identifiers, making it impossible to trace errors across distributed operations or correlate log entries for a single request
  When valid: Only appropriate for single-threaded applications with no concurrent request handling
- Embed configuration values directly in code or use configuration files (rejected)
  Rejected because: Hardcoded configuration prevents environment-specific deployments and requires code changes or file management for configuration updates, violating twelve-factor app principles
  When valid: Acceptable for static configuration that never varies across environments, such as algorithm constants
- Use unstructured string-based logging with manual formatting (rejected)
  Rejected because: Unstructured logs are difficult to parse programmatically, preventing automated analysis, alerting, and integration with log aggregation systems
  When valid: May be acceptable for local development debugging where human readability is prioritized over machine parsing

## Risks

- Sensitive credentials or API keys may be accidentally logged if context objects or error messages contain secret values
  Mitigation: Implement log sanitization middleware that redacts known secret patterns and audit logging code for potential secret exposure. Use dedicated secret management patterns that prevent secrets from entering log pipelines.
  Owner: Security team with engineering team implementation
- Missing or misconfigured environment variables may cause runtime failures that are difficult to diagnose if not validated at startup
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages. Document all required environment variables in deployment guides.
  Owner: Engineering team
- Excessive structured logging may introduce performance overhead or generate high log volumes that increase storage costs
  Mitigation: Implement log level controls and sampling strategies for high-volume operations. Monitor log volume metrics and adjust verbosity based on operational needs.
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
- Establish a consistent pattern for extracting logger instances from context objects at the beginning of each function that may log errors, ensuring context propagation is maintained throughout the call chain
- Create a centralized configuration module that validates required environment variables at application startup and provides typed accessors to prevent runtime configuration errors
- Implement structured logging field conventions that define standard field names for common attributes such as error types, HTTP status codes, request identifiers, and operation names to ensure consistency across services

## Continuation Context


Verify commands:
- Discover the project's code analysis tooling and execute static analysis to identify logging call sites that do not use context-aware logging patterns
- Discover the project's testing framework and run integration tests that verify environment variable validation occurs at startup and produces appropriate error messages for missing configuration
- Discover the project's log aggregation configuration and verify that structured log fields are properly indexed and queryable for error correlation

Accept when:
- All error logging call sites use context-aware structured logging APIs and preserve request correlation identifiers
- All required environment variables are validated at application startup with clear error messages for missing or invalid values
- Log entries contain sufficient structured fields to enable automated error correlation and analysis across service boundaries

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline that detects non-context-aware logging patterns
- Verified by: Code review checklist items that verify context propagation and structured logging field usage
- Verified by: Integration test suite that validates environment variable handling and error logging behavior
- Violation handling: Static analysis violations block pull request merge until resolved
- Violation handling: Code review findings require remediation before approval
- Violation handling: Production incidents caused by inadequate error logging trigger retrospectives and pattern reinforcement
- Exception process: Exception requests must be submitted to architecture review board with documented justification
- Exception process: Approved exceptions require tracking issue with remediation timeline
- Exception process: Exception status reviewed quarterly with progress updates required