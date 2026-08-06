# Retrieve Secrets from Environment Variables in Integration Test Contexts: Http Client Constructors

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Integration test contexts require authenticated HTTP clients to interact with external services such as data ingestion APIs and task scheduling systems
- Credentials for external service authentication are retrieved at runtime using environment variable lookups rather than configuration files or secret management services
- Multiple distinct credentials are required across different integration boundaries, including API keys for third-party services and service account credentials for cloud platform authentication
- The pattern co-occurs with HTTP client construction, request body assembly, and authorization header injection, indicating secrets are consumed immediately after retrieval

## Problem Statement

Integration test contexts and external client initialization require authenticated access to third-party APIs and cloud services, but the mechanism for retrieving these credentials must balance ease of local development, CI environment compatibility, and security isolation without introducing hard-coded secrets or complex secret management infrastructure during testing phases.

## Decision

1. MUST: HTTP client constructors and external service adapters that require authentication SHALL accept secrets as parameters rather than performing environment lookups internally

## Policy Block

- MUST HTTP client constructors and external service adapters that require authentication SHALL accept secrets as parameters rather than performing environment lookups internally

In scope:
- Integration test initialization code that constructs HTTP clients for external services
- Service adapter constructors that require API keys or bearer tokens for authentication
- Cloud platform client initialization requiring service account credentials
- Task scheduling client setup requiring authentication secrets

Out of scope:
- Production application runtime configuration
- Unit test contexts that use mocked external dependencies
- Local development server configuration
- Secrets used for database connection strings or internal service authentication

## Rationale

- Environment variables provide a standard, platform-agnostic mechanism for injecting secrets into integration test contexts that works consistently across local development, CI pipelines, and containerized test environments
- The evidence shows multiple distinct secrets being retrieved for different external services, indicating a pattern of per-service credential isolation rather than shared authentication mechanisms
- Immediate consumption of secrets after retrieval, as evidenced by direct usage in HTTP client construction and authorization header assembly, minimizes the window of exposure and reduces the risk of accidental logging or persistence
- The co-occurrence with integration test body assembly and HTTP request construction indicates this pattern specifically serves test contexts rather than production runtime configuration

## Consequences

Positive:
- Integration tests can be executed in any environment that supports environment variable injection without requiring secret management infrastructure
- Credential rotation requires only environment variable updates without code changes or redeployment
- Per-service credential isolation enables fine-grained access control and audit trails for external service interactions
- CI pipeline integration is simplified as most CI systems provide native environment variable injection mechanisms

Negative:
- Environment variables are visible to all processes running under the same user context, creating potential exposure risk in shared development environments
- No built-in secret rotation, versioning, or audit trail capabilities compared to dedicated secret management services
- Developers must manually manage and synchronize environment variable configuration across local development, CI, and test environments
- Missing or misconfigured environment variables result in runtime failures rather than compile-time or configuration validation errors

## Alternatives

- Use a dedicated secret management service with SDK integration to retrieve secrets at runtime (rejected)
  Rejected because: Introduces additional infrastructure dependencies and complexity for integration test contexts where environment variable injection is already standard practice in CI systems
  When valid: Production runtime environments requiring audit trails, automatic rotation, and centralized secret governance
- Store encrypted secrets in configuration files and decrypt at runtime using a master key (rejected)
  Rejected because: Requires key management infrastructure and adds decryption overhead while still requiring secure distribution of the master key, often through environment variables
  When valid: Scenarios requiring offline secret access or when environment variable injection is not supported by the deployment platform
- Use mock credentials and stub external service responses in all integration tests (rejected)
  Rejected because: Eliminates true integration testing by removing actual external service interaction, reducing confidence in authentication flow correctness and API contract compliance
  When valid: Unit test contexts or when external service availability is unreliable and contract testing is sufficient

## Risks

- Environment variables containing secrets may be inadvertently logged or exposed through error messages, stack traces, or debugging output
  Mitigation: Implement structured logging with secret redaction filters and validate that error handling code does not include environment variable values in exception messages
  Owner: engineering team
- Developers may commit environment variable configuration files containing actual secrets to version control
  Mitigation: Maintain template configuration files with placeholder values, document the required environment variables separately, and configure version control ignore rules to exclude environment files
  Owner: engineering team
- Integration tests may fail silently or produce misleading results if environment variables are missing but default to empty strings
  Mitigation: Implement explicit validation of required environment variables during test setup and fail fast with clear error messages identifying missing credentials
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
- Create a template environment configuration file documenting all required environment variable names with descriptive comments explaining their purpose, expected format, and which external services they authenticate to
- Implement a test setup helper function that validates the presence of all required environment variables and returns a structured error listing missing credentials before any HTTP client construction occurs
- When constructing authorization headers from environment-sourced secrets, use formatted string construction to ensure consistent bearer token or API key header structure across all external service clients

## Continuation Context


Verify commands:
- Discover the project's test execution script and invoke it with integration test selection flags to verify that test setup code retrieves credentials from environment variables
- Discover the project's static analysis or linting configuration and execute it to verify that no hard-coded credential strings exist in integration test files
- Discover the project's environment variable documentation or template configuration and verify that all secrets referenced in integration test code are documented with their expected names and formats

Accept when:
- All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions
- Test execution fails with clear error messages identifying missing environment variables when required credentials are not present
- No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed

## Enforcement

- Verified by: Code review verification that integration test setup code uses environment variable retrieval for all secrets
- Verified by: Static analysis rules detecting hard-coded credential patterns in test files
- Verified by: CI pipeline validation that integration tests fail appropriately when required environment variables are not configured
- Violation handling: Pull requests containing hard-coded secrets in integration test code are blocked from merging
- Violation handling: Integration tests that do not validate required environment variables before client construction are flagged for refactoring
- Violation handling: Secret values found in logs or error messages trigger immediate incident response and credential rotation
- Exception process: Exceptions for alternative secret retrieval mechanisms require architecture review and documentation of the specific integration test scenario that cannot use environment variables
- Exception process: Temporary hard-coded test credentials for local development must be clearly marked as non-production, use dedicated test accounts with minimal privileges, and be documented in test setup instructions