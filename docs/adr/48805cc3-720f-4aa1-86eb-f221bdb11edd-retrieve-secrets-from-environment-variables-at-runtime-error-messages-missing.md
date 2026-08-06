# Retrieve Secrets from Environment Variables at Runtime: Error Messages Missing

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Integration tests require access to external services that authenticate using API keys, bearer tokens, and service account credentials
- Test environments must isolate secrets from source code to prevent accidental commits and enable environment-specific configuration
- Runtime configuration sources use environment variable lookups to retrieve authentication material for HTTP clients and cloud service clients
- Multiple services coordinate secret retrieval through a consistent pattern of reading environment variables at initialization time

## Problem Statement

Integration tests and runtime clients need to authenticate with external services and cloud providers without embedding secrets in source code or configuration files, while maintaining the ability to run tests in multiple environments with different credentials.

## Decision

1. SHOULD: Error messages for missing environment variables SHOULD identify the specific variable name without exposing the secret value

## Policy Block

- SHOULD Error messages for missing environment variables SHOULD identify the specific variable name without exposing the secret value

In scope:
- Integration test suites that connect to external HTTP APIs
- Client initialization code that authenticates with cloud service providers
- HTTP request construction that requires bearer tokens or API keys
- Service account credential loading for cloud platform authentication

Out of scope:
- Unit tests that use mocked clients and do not require real credentials
- Local development configuration files that may use alternative secret management
- Production deployment secret management systems that inject environment variables

## Rationale

- The IR evidence shows consistent use of environment variable retrieval for multiple secret types including API keys, service URLs, and cloud service account credentials across integration test code
- Environment variables provide a standard mechanism for injecting secrets at runtime that works across local development, CI/CD pipelines, and production deployments
- This pattern separates secret management from code management, enabling different credentials per environment without code changes
- The detection of this pattern in client initialization and HTTP request construction code indicates it is the established approach for external service authentication

## Consequences

Positive:
- Secrets remain isolated from source control, reducing the risk of accidental credential exposure
- Integration tests can run in multiple environments with environment-specific credentials without code modification
- The pattern uses standard library functions requiring no additional dependencies for secret retrieval
- Credential rotation can occur through environment variable updates without recompiling or redeploying code

Negative:
- Environment variables are visible to all processes running under the same user context, creating potential information disclosure risks
- Missing or misconfigured environment variables cause runtime failures rather than compile-time errors
- Debugging authentication failures requires access to the runtime environment to inspect variable presence and values
- Each deployment environment requires separate secret provisioning and environment variable configuration

## Alternatives

- Embed secrets in configuration files stored outside version control (rejected)
  Rejected because: Configuration files require file system access and deployment coordination, while environment variables are universally supported by test runners, CI systems, and container orchestrators
  When valid: When running in environments that do not support environment variable injection but do support secure file mounting
- Use a dedicated secret management service with SDK integration (rejected)
  Rejected because: Adds external service dependency and SDK complexity for integration test scenarios where environment variables provide sufficient isolation and flexibility
  When valid: When secrets require audit logging, automatic rotation, or fine-grained access control beyond environment-level isolation
- Pass secrets as command-line arguments to test executables (rejected)
  Rejected because: Command-line arguments are visible in process listings and shell history, creating greater exposure risk than environment variables
  When valid: Never recommended for secret material due to process visibility concerns

## Risks

- Environment variables may be logged or exposed through error messages, stack traces, or debugging output
  Mitigation: Implement error handling that reports missing variables without exposing values, and configure logging systems to redact environment variable contents
  Owner: engineering team
- Integration tests may fail silently or with unclear errors when required environment variables are not set
  Mitigation: Add explicit validation at test initialization that checks for required variables and provides clear error messages identifying missing configuration
  Owner: engineering team
- Different environments may use inconsistent variable naming conventions, causing configuration drift
  Mitigation: Document all required environment variables in a central location and validate naming consistency across development, CI, and production environments
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
- Client initialization code should retrieve environment variables once during construction and store them in private fields, avoiding repeated lookups during request execution
- Integration test setup should document all required environment variables in test documentation or README files, including their purpose and expected format
- Consider implementing a validation function that checks for required environment variables at test suite initialization and fails fast with clear error messages before attempting authentication

## Continuation Context


Verify commands:
- Discover the project's test execution script and run integration tests with required environment variables set to verify authentication succeeds
- Discover the project's static analysis or linting configuration and verify it includes checks for hardcoded secrets or credentials in source files
- Discover the project's client initialization code and verify all external service clients retrieve authentication material from environment variables

Accept when:
- All integration tests that authenticate with external services retrieve credentials exclusively from environment variables
- Static analysis confirms no authentication secrets are hardcoded in source files or committed to version control
- Client initialization code documents the required environment variable names and validates their presence before use

## Enforcement

- Verified by: Code review verification that new client code retrieves secrets from environment variables
- Verified by: Static analysis scanning for hardcoded credentials in source files
- Verified by: Integration test execution in CI that validates environment variable configuration
- Violation handling: Code review rejection for pull requests that hardcode secrets or bypass environment variable retrieval
- Violation handling: CI pipeline failure when static analysis detects potential hardcoded credentials
- Violation handling: Security incident response process for any secrets accidentally committed to version control
- Exception process: Document the specific use case requiring an alternative secret retrieval mechanism
- Exception process: Obtain security team approval for alternative approaches that maintain equivalent isolation guarantees
- Exception process: Record the exception in architecture documentation with justification and scope limitations