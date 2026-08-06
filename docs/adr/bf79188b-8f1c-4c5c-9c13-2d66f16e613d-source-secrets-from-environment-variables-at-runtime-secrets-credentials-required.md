# Source Secrets from Environment Variables at Runtime: Secrets Credentials Required

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system requires external service credentials including API keys, service account keys, and authentication tokens to interact with third-party services at runtime
- Multiple components need access to sensitive configuration values such as TINYBIRD_URL, CRON_SECRET, and GCP service account credentials (private key, client email, key ID, project ID)
- The runtime environment provides a standard mechanism for injecting configuration without hardcoding secrets in source code or version control
- The pattern appears in both HTTP client initialization and task processing workflows where external service authentication is required

## Problem Statement

Services require secure access to credentials and configuration values for external integrations without embedding sensitive data in source code, while maintaining flexibility across deployment environments and adhering to twelve-factor application principles for configuration management.

## Decision

1. MUST: All secrets and credentials required for external service authentication MUST be sourced from environment variables at runtime using the standard library environment access mechanism

## Policy Block

- MUST All secrets and credentials required for external service authentication MUST be sourced from environment variables at runtime using the standard library environment access mechanism

In scope:
- All application components that authenticate to external HTTP APIs
- Service initialization code that constructs authenticated clients
- Task handlers and request processors that require credentials for third-party service integration
- Configuration loading modules that prepare runtime context for authenticated operations

Out of scope:
- Development and testing environments where mock credentials or test fixtures are used
- Build-time configuration that does not involve runtime secrets
- Public configuration values that do not require confidentiality
- Client-side code that does not have access to server environment variables

Exceptions:
- EX-001: Local development environments where developers use personal test credentials
- EX-002: Integration tests that require real credentials to validate external service contracts

## Rationale

- Environment variables provide a standard, platform-agnostic mechanism for injecting secrets at runtime that separates configuration from code and aligns with twelve-factor application principles
- The evidence shows consistent use of environment variable access for multiple credential types across different service boundaries, indicating an established pattern for secrets management
- Sourcing secrets from the environment enables secure deployment practices where secrets are managed by orchestration platforms, secret managers, or CI/CD systems without requiring code changes
- The pattern supports environment-specific configuration allowing the same codebase to operate across development, staging, and production with different credentials

## Consequences

Positive:
- Secrets remain external to source code and version control, reducing the risk of accidental credential exposure through repository access
- Deployment flexibility increases as the same binary or container image can be deployed to multiple environments with different credentials injected at runtime
- Credential rotation becomes operationally simpler as secrets can be updated in the environment without requiring code changes or redeployment
- The pattern integrates naturally with container orchestration platforms and cloud secret management services that inject environment variables

Negative:
- Environment variables may be visible to all processes running under the same user context, creating potential lateral exposure risks in shared environments
- Debugging and troubleshooting become more complex as configuration is not visible in the codebase and requires access to deployment environment
- Missing or misconfigured environment variables may only be detected at runtime rather than at build time, potentially causing production failures
- Environment variable limits on some platforms may constrain the size or number of secrets that can be injected, particularly for large certificate or key files

## Alternatives

- Store secrets in configuration files encrypted at rest and decrypt at application startup (rejected)
  Rejected because: Requires key management for decryption keys, adds complexity to deployment process, and still requires secure injection of decryption credentials
  When valid: When secrets are too large for environment variables or when regulatory requirements mandate encrypted storage of credentials at rest
- Fetch secrets from a dedicated secret management service at runtime using service identity authentication (deferred)
  Rejected because: Adds external dependency and network calls during initialization, increases operational complexity, but provides better audit trails and dynamic rotation
  When valid: When operating at scale with many services requiring centralized secret rotation, audit logging, and fine-grained access control
- Use mounted secret volumes provided by container orchestration platforms (deferred)
  Rejected because: Ties deployment to specific orchestration platforms and requires file system access patterns instead of environment access
  When valid: When secrets exceed environment variable size limits or when platform-native secret injection is preferred over environment variables

## Risks

- Environment variables may be inadvertently logged or exposed through process inspection tools, error messages, or debugging output
  Mitigation: Implement logging filters that redact known secret environment variable names, avoid echoing environment in error messages, and restrict process inspection capabilities in production
  Owner: Security team and platform engineering
- Missing environment variables may cause runtime failures that are difficult to diagnose without clear error messages indicating which secrets are absent
  Mitigation: Implement startup validation that checks for required environment variables and fails fast with clear messages listing missing configuration before attempting service operations
  Owner: Engineering team
- Credential rotation requires coordinated updates across all deployment environments and may cause service disruption if not synchronized properly
  Mitigation: Implement graceful credential refresh mechanisms, support overlapping validity periods for credentials during rotation, and document rotation procedures with rollback plans
  Owner: Operations team and security team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implement centralized configuration loading that validates all required environment variables at application startup before initializing service clients, providing clear error messages that list missing variables without exposing their values
- Use structured logging with automatic redaction of fields matching secret patterns to prevent accidental credential exposure in logs while maintaining debuggability of configuration issues
- Document the complete list of required environment variables for each service component in deployment documentation, including expected format, purpose, and whether they are required or optional
- Consider implementing a configuration validation mode that checks environment variable presence and format without starting the full application, enabling pre-deployment validation

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis tooling to scan source files for hardcoded credential patterns or secret strings
- Locate the project's test suite and run integration tests that validate environment variable loading and missing-secret error handling
- Identify the project's linting or security scanning configuration and verify it includes rules that detect hardcoded secrets or credentials in source code

Accept when:
- Static analysis confirms no hardcoded credentials, API keys, or secret strings are present in source files committed to version control
- All service initialization code successfully retrieves required secrets from environment variables and fails fast with clear error messages when critical variables are missing
- Integration tests demonstrate that services correctly authenticate to external APIs using environment-sourced credentials and handle missing or invalid credentials gracefully

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline scanning for hardcoded secrets and credential patterns
- Verified by: Code review checklist requiring verification that new external service integrations use environment variables for credentials
- Verified by: Security scanning tools that detect exposed secrets in source code, configuration files, and container images
- Violation handling: CI pipeline fails and blocks merge if static analysis detects hardcoded credentials or secret patterns in code changes
- Violation handling: Security team notification triggered for any detected credential exposure with immediate incident response
- Violation handling: Mandatory credential rotation required if secrets are discovered in version control history, with post-incident review
- Exception process: Exception requests must be submitted to security team with documented justification and alternative mitigation controls
- Exception process: Approved exceptions require time-limited approval with mandatory review period and compensating security controls
- Exception process: All exceptions must be documented in security review records with clear scope, duration, and conditions for removal