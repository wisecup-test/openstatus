# Adopt bcryptjs for API Key Hashing with crypto.randomBytes Generation: Before Implementing Credential

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all API key generation, hashing, and verification operations within the authentication subsystem.

## Context

- The system requires secure API key generation and storage for authenticating programmatic access to public APIs
- API keys must be cryptographically random to prevent prediction attacks and must be stored as irreversible hashes to protect against credential theft
- The authentication module exposes public contracts for generating, hashing, and verifying API keys, indicating a standardized credential management interface
- The pattern uses bcryptjs for adaptive hashing with configurable work factors and crypto.randomBytes for entropy generation, balancing security with performance requirements

## Problem Statement

API key authentication requires cryptographically secure token generation and storage that prevents both prediction attacks during generation and credential recovery attacks if the storage layer is compromised, while maintaining acceptable performance for verification operations at scale.

## Decision

1. MUST: Before implementing credential hashing, the consumer MUST discover the project's dependency lock artifact, resolve the exact installed version of the bcrypt library, and verify API compatibility against that version's official documentation

## Policy Block

- MUST Before implementing credential hashing, the consumer MUST discover the project's dependency lock artifact, resolve the exact installed version of the bcrypt library, and verify API compatibility against that version's official documentation

In scope:
- All API key generation operations for public API authentication
- All API key storage operations in credential databases
- All API key verification operations during authentication flows
- Public authentication contracts exposed to application layers

Out of scope:
- User password hashing, which may have different work factor requirements
- Session token generation, which may use different entropy sources
- OAuth token handling, which follows separate specifications
- Internal service-to-service authentication using mutual TLS or other mechanisms

Exceptions:
- EXC-001: Testing environments require deterministic API key generation for fixture data
- EXC-002: Migration scenarios require temporary dual-verification during credential rotation

## Rationale

- The evidence shows explicit use of crypto.randomBytes(16) for token generation and bcrypt.hash with work factor 10, demonstrating a deliberate security architecture for credential management
- The public contract exports (generateApiKey, hashApiKey, verifyApiKeyHash) indicate this pattern is intended as a reusable authentication primitive across the application
- bcryptjs provides adaptive hashing that can scale work factors as computational power increases, while crypto.randomBytes ensures CSPRNG-quality entropy for unpredictable tokens
- The shouldUpdateLastUsed contract suggests the system tracks API key usage patterns for security monitoring without blocking authentication flows

## Consequences

Positive:
- API keys are cryptographically unpredictable, preventing enumeration and prediction attacks
- Stored credentials are protected by adaptive one-way hashing, limiting damage from database compromise
- Constant-time verification prevents timing side-channel attacks during authentication
- Public contracts enable consistent API key handling across multiple application modules

Negative:
- bcrypt hashing introduces computational latency during both credential creation and verification operations
- Work factor tuning requires balancing security requirements against authentication throughput and user experience
- The pattern couples the authentication layer to specific cryptographic library implementations, requiring careful version management and migration planning

## Alternatives

- Use PBKDF2 with HMAC-SHA256 for API key hashing instead of bcrypt (rejected)
  Rejected because: bcrypt provides built-in salt generation and adaptive work factors with broader security community validation for credential storage, while PBKDF2 requires explicit salt management and iteration count tuning
  When valid: Valid for systems with existing PBKDF2 infrastructure or regulatory requirements specifying NIST-approved algorithms
- Store API keys in plaintext or reversibly encrypted form for customer service recovery (rejected)
  Rejected because: Reversible storage creates a single point of compromise where database access yields all credentials, violating defense-in-depth principles
  When valid: Never valid for production authentication credentials
- Use Argon2id for API key hashing with memory-hard properties (deferred)
  Rejected because: Argon2id offers superior resistance to GPU and ASIC attacks but requires evaluation of memory constraints and library maturity in the target runtime
  When valid: Valid for high-security environments with sufficient memory resources and when protection against specialized hardware attacks is prioritized

## Risks

- bcrypt work factor 10 may become insufficient as computational power increases, weakening protection against offline brute-force attacks
  Mitigation: Establish periodic security review cycle to evaluate work factor adequacy and implement credential rehashing on authentication to transparently upgrade existing hashes
  Owner: Security team
- High authentication throughput may create CPU bottlenecks during bcrypt verification, impacting API response times
  Mitigation: Implement authentication caching with short TTLs, monitor verification latency metrics, and establish performance baselines before scaling
  Owner: Engineering team
- Dependency on bcryptjs creates supply chain risk if the library is compromised or abandoned
  Mitigation: Pin exact versions in lock artifacts, monitor security advisories, maintain abstraction layer for credential operations to enable future migration
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
- Generate API keys by encoding random bytes to URL-safe base64 or hexadecimal format to ensure compatibility with HTTP headers and query parameters
- Return the plaintext API key to the user exactly once during generation, as the stored hash cannot be reversed for recovery
- Implement shouldUpdateLastUsed with asynchronous or background processing to avoid blocking the authentication response path
- Consider implementing rate limiting on API key verification attempts to mitigate online brute-force attacks independent of hash strength

## Continuation Context


Verify commands:
- Discover the project's test execution configuration and run the authentication module's test suite to verify API key generation produces unique tokens
- Discover the project's static analysis or linting configuration and verify that credential hashing operations use the required work factor
- Discover the project's dependency verification tooling and confirm the cryptographic libraries are pinned to exact versions in the lock artifact

Accept when:
- All API key generation tests pass, demonstrating cryptographically random token creation with sufficient entropy
- All API key verification tests pass, including constant-time comparison validation and hash mismatch handling
- Static analysis confirms no plaintext API key storage and all credential operations use the approved hashing interface
- Dependency verification confirms cryptographic libraries are locked to specific versions and match security baseline requirements

## Enforcement

- Verified by: Automated test suite execution in continuous integration validating API key generation, hashing, and verification contracts
- Verified by: Static code analysis scanning for plaintext credential storage or weak random number generation
- Verified by: Security-focused code review checklist requiring verification of work factors and entropy sources
- Verified by: Dependency scanning tools validating cryptographic library versions against known vulnerabilities
- Violation handling: CI pipeline blocks merges when authentication tests fail or static analysis detects insecure credential handling
- Violation handling: Security team notification triggered for any plaintext credential storage or weak randomness detection
- Violation handling: Automated rollback of deployments that introduce credential storage vulnerabilities
- Violation handling: Incident response process activated for production violations with immediate credential rotation
- Exception process: Exception requests must be submitted to security team with architectural justification and risk assessment
- Exception process: Approved exceptions require documented compensating controls and time-bound remediation plans
- Exception process: All exceptions logged in security decision register with approval chain and review dates
- Exception process: Exceptions reviewed quarterly with automatic expiration requiring re-approval