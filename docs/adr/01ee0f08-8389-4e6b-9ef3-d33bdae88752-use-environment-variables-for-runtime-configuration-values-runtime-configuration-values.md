# Use Environment Variables for Runtime Configuration Values: Runtime Configuration Values

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase requires runtime configuration values that vary across deployment environments, including service endpoints, authentication credentials, and project identifiers.
- Configuration values are retrieved at runtime using the standard library environment variable accessor, enabling deployment-time injection without code modification.
- Multiple services coordinate through shared configuration sources, requiring a consistent mechanism for retrieving environment-specific parameters.
- The pattern separates configuration data from application logic, allowing the same binary to operate in different environments based on injected values.

## Problem Statement

Services require runtime configuration values that differ across environments (development, staging, production) without embedding sensitive credentials or environment-specific endpoints in source code or compiled binaries. The system must retrieve these values at startup or request time while maintaining clear boundaries between configuration sources and application logic.

## Decision

1. MUST: Runtime configuration values that vary across deployment environments MUST be retrieved from environment variables using the standard library accessor.

## Policy Block

- MUST Runtime configuration values that vary across deployment environments MUST be retrieved from environment variables using the standard library accessor.

In scope:
- Service endpoint URLs and base paths
- Authentication credentials and API keys
- Service account identifiers and project IDs
- Environment-specific feature flags
- External service configuration parameters

Out of scope:
- Application constants that do not vary across environments
- Type definitions and interface contracts
- Business logic and algorithmic parameters
- Build-time configuration and compilation flags

## Rationale

- The evidence shows consistent use of environment variable retrieval across multiple services for URLs, credentials, and project identifiers, indicating an established pattern for runtime configuration.
- Separating configuration from code enables the same binary to operate in multiple environments and prevents credential exposure in version control systems.
- Environment variables provide a standard, platform-agnostic mechanism supported by container orchestration, cloud platforms, and local development environments.
- The pattern aligns with twelve-factor application principles for configuration management and enables secure credential injection through deployment tooling.

## Consequences

Positive:
- Configuration values can be changed without recompiling or redeploying application code.
- Sensitive credentials remain outside version control and can be managed through secure secret management systems.
- The same application binary can operate in multiple environments based on injected configuration.
- Configuration sources are explicit and discoverable through environment inspection rather than scattered across code.

Negative:
- Missing or misconfigured environment variables may cause runtime failures that are not detected until deployment.
- Environment variable management adds operational complexity across multiple deployment targets.
- Debugging configuration issues requires access to deployment environment settings rather than inspecting code alone.
- Type safety and validation must be implemented explicitly since environment variables are untyped strings.

## Alternatives

- Embed configuration values directly in source code with conditional compilation for different environments (rejected)
  Rejected because: Embedding credentials in source code creates security risks through version control exposure and requires recompilation for configuration changes, violating separation of concerns between code and configuration.
  When valid: Only acceptable for true application constants that never vary across environments and contain no sensitive data.
- Use configuration files deployed alongside binaries with environment-specific content (rejected)
  Rejected because: Configuration files require file system access and deployment coordination, adding complexity compared to environment variable injection, and may expose credentials in file system storage.
  When valid: Appropriate for complex structured configuration that exceeds the simplicity of key-value pairs or when configuration must be updated without process restart.
- Retrieve configuration from a centralized configuration service at runtime (rejected)
  Rejected because: Introduces additional service dependencies and network calls during initialization, increasing failure modes and latency, though the pattern may complement environment variables for dynamic configuration.
  When valid: Useful for configuration that changes frequently during runtime or must be coordinated across many service instances simultaneously.

## Risks

- Missing or incorrectly named environment variables cause runtime failures in production environments that were not detected in earlier testing stages.
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear error messages. Document all required variables and maintain environment-specific configuration checklists.
  Owner: engineering team
- Environment variables containing sensitive credentials may be logged, exposed through process inspection, or leaked in error messages.
  Mitigation: Implement logging filters to redact credential values. Avoid echoing environment variable contents in error messages. Use process isolation and access controls to restrict environment inspection.
  Owner: engineering team
- Configuration drift across environments leads to inconsistent behavior when environment variables are set to different values or use different naming conventions.
  Mitigation: Maintain a canonical list of environment variable names and expected formats. Use infrastructure-as-code to manage environment variable configuration consistently across deployment targets.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Retrieve environment variables during service initialization or client construction to enable fail-fast behavior when required configuration is missing, rather than deferring retrieval until first use.
- Implement explicit validation for required environment variables, checking for presence and non-empty values, and provide clear error messages identifying which variables are missing or invalid.
- Consider implementing a configuration struct or object that encapsulates all environment variable retrieval and validation in a single location, providing a typed interface to the rest of the application.
- Document all required and optional environment variables in deployment documentation, including expected formats, example values for non-sensitive parameters, and the impact of missing values.

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the standard library module used for environment variable access, then locate test files that verify configuration retrieval behavior.
- Identify the project's static analysis or linting configuration and execute the configured checks to verify that no hardcoded credentials or environment-specific values appear in source code.
- Locate the project's deployment or infrastructure configuration and verify that all environment variables referenced in application code are documented and provisioned in deployment targets.

Accept when:
- All runtime configuration values that vary across environments are retrieved from environment variables using the standard library accessor.
- No sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- Services validate required environment variables during initialization and fail with clear error messages when configuration is missing.
- All required environment variables are documented in deployment documentation with expected formats and example values.

## Enforcement

- Verified by: Code review verification that configuration values are sourced from environment variables rather than embedded in code.
- Verified by: Static analysis checks for hardcoded credentials or environment-specific values in source files.
- Verified by: Integration tests that verify service initialization behavior with missing or invalid environment variables.
- Verified by: Deployment checklist verification that all required environment variables are provisioned in target environments.
- Violation handling: Code review rejection for pull requests that embed credentials or environment-specific configuration in source code.
- Violation handling: Build failure when static analysis detects hardcoded sensitive values or configuration that should be externalized.
- Violation handling: Deployment rollback when services fail initialization due to missing required environment variables.
- Violation handling: Security incident response when credentials are discovered in version control history.
- Exception process: Exceptions for embedding non-sensitive application constants require architectural review to confirm the values truly do not vary across environments.
- Exception process: Temporary hardcoding during local development must be removed before code review and must never include actual production credentials.
- Exception process: Alternative configuration mechanisms require architectural decision review to document tradeoffs and ensure consistent patterns across the codebase.