# Adopt Cryptographic API Key Generation and Bcrypt Hashing for Public API Authentication: Before Implementing Credential

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system exposes public API contracts requiring secure authentication mechanisms to protect data access and prevent unauthorized usage
- API keys serve as bearer tokens for service-to-service and client-to-service authentication, necessitating cryptographically secure generation to prevent prediction or collision attacks
- Credential storage must protect against database compromise by storing only irreversible hashes rather than plaintext tokens
- The codebase implements a complete API key lifecycle including generation, hashing, verification, and usage tracking through dedicated utility functions

## Problem Statement

Public API authentication requires a secure token generation and storage mechanism that balances usability with protection against credential theft, brute-force attacks, and database compromise while maintaining verifiable authentication and usage tracking capabilities.

## Decision

1. MUST: Before implementing credential hashing, consumers must discover the project's dependency lock artifact, resolve the exact installed version of the adaptive hashing library, and verify API compatibility with that version's official documentation

## Policy Block

- MUST Before implementing credential hashing, consumers must discover the project's dependency lock artifact, resolve the exact installed version of the adaptive hashing library, and verify API compatibility with that version's official documentation

In scope:
- All public API endpoints requiring authentication
- Service-to-service authentication mechanisms
- Client credential generation and management workflows
- API key lifecycle operations including creation, verification, and revocation

Out of scope:
- User password authentication flows
- OAuth or third-party identity provider integrations
- Session-based authentication for web applications
- Internal service mesh authentication using mutual TLS

## Rationale

- The IR evidence shows explicit use of cryptographic random byte generation and adaptive hashing libraries, indicating a deliberate security-first approach to credential management
- The pattern exports four distinct public contracts for generation, hashing, verification, and usage tracking, demonstrating architectural separation between credential lifecycle phases
- Adaptive hashing with configurable work factors provides future-proof protection against increasing computational power while maintaining backward compatibility with existing stored hashes
- The detection of usage tracking logic suggests operational requirements for rate limiting and audit trails beyond basic authentication

## Consequences

Positive:
- Cryptographically secure token generation prevents prediction and collision attacks even with knowledge of the generation algorithm
- Adaptive hashing with work factors protects stored credentials against offline brute-force attacks following database compromise
- Clear separation of generation, hashing, and verification functions enables independent testing and security auditing of each phase
- Usage tracking support enables rate limiting, anomaly detection, and compliance audit trails

Negative:
- Adaptive hashing introduces computational overhead on every authentication request, requiring capacity planning for high-throughput APIs
- Work factor configuration requires ongoing tuning as hardware capabilities evolve to maintain security without degrading performance
- API key rotation and revocation workflows require additional implementation beyond the core generation and verification primitives
- Lost or compromised keys cannot be recovered and require regeneration, potentially disrupting client integrations

## Alternatives

- Use JWT tokens with asymmetric signing for stateless authentication (rejected)
  Rejected because: JWT tokens expose claims and metadata in the token payload, increasing information disclosure risk, and require key rotation infrastructure for signing keys rather than simple per-client credential revocation
  When valid: When authentication requires embedding scopes, expiration, or other claims that must be verifiable without database lookup
- Store API keys encrypted with application-level encryption keys rather than hashed (rejected)
  Rejected because: Encryption is reversible and requires secure key management infrastructure; compromise of the encryption key exposes all stored credentials, whereas hashing provides irreversible protection
  When valid: When the system requires the ability to retrieve and display existing API keys to users after initial generation
- Implement OAuth 2.0 client credentials flow for machine-to-machine authentication (deferred)
  Rejected because: OAuth adds protocol complexity and token refresh infrastructure overhead that may not be justified for simple API key use cases
  When valid: When integrating with third-party services that require OAuth compliance or when token expiration and refresh capabilities are required

## Risks

- Work factor misconfiguration could result in either insufficient protection against brute-force attacks or excessive authentication latency impacting API performance
  Mitigation: Establish baseline performance testing for authentication operations and document work factor selection rationale with periodic review cycles aligned to hardware capability trends
  Owner: Security Engineering Team
- API key transmission over insecure channels exposes credentials to interception despite secure storage
  Mitigation: Enforce transport-layer security for all API endpoints and implement detection for plaintext credential transmission attempts in monitoring systems
  Owner: Platform Engineering Team
- Lack of key rotation policies allows compromised credentials to remain valid indefinitely
  Mitigation: Implement expiration metadata in credential storage and automated notification workflows for approaching expiration dates
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Generate API keys with minimum 128-bit entropy from cryptographic random sources; encode in URL-safe format for transmission and storage in HTTP headers
- Configure adaptive hashing work factors based on authentication latency requirements; target 100-250ms per hash operation as baseline for acceptable user experience
- Implement usage tracking with configurable throttle windows to prevent database write amplification on high-frequency authentication; consider caching last-used timestamps with eventual consistency

## Continuation Context


Verify commands:
- Discover the project's test execution configuration and run the test suite covering credential generation, hashing, and verification functions
- Locate the project's static analysis or linting configuration and execute security-focused rules checking for plaintext credential storage
- Identify the project's dependency scanning tooling and verify no known vulnerabilities exist in cryptographic or hashing dependencies

Accept when:
- All tests for API key generation, hashing, verification, and usage tracking functions pass with 100% coverage of security-critical paths
- Static analysis confirms no plaintext credential persistence and all random generation uses cryptographically secure sources
- Dependency scanning reports zero high or critical vulnerabilities in cryptographic libraries

## Enforcement

- Verified by: Automated test suite execution in continuous integration pipeline covering all credential lifecycle functions
- Verified by: Static analysis and security scanning integrated into pull request validation workflows
- Verified by: Periodic security audits reviewing credential storage patterns and hashing configuration
- Violation handling: Pull requests introducing plaintext credential storage or weak random generation are automatically blocked by CI checks
- Violation handling: Security scanning alerts trigger immediate review and remediation workflows for vulnerable dependency versions
- Violation handling: Code review guidelines require explicit verification of cryptographic library usage and work factor configuration
- Exception process: Exceptions require written justification documenting compensating controls and risk acceptance from security engineering
- Exception process: Temporary exceptions must include remediation timeline and tracking issue linked to the exception approval
- Exception process: All exceptions are reviewed quarterly and automatically expire after six months unless explicitly renewed