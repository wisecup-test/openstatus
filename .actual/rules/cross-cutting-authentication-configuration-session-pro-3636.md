---
version: 2
name: cross-cutting-authentication-configuration-session-pro-3636
description: "Adoption of next-auth for Authentication and Session Management: Authentication Configuration Session Provider Initialization Centralized"
metadata:
  actual:
    adr_id: 3636ca9f-5ace-44dd-8e62-7e5b91369818
    status: accepted
    candidate_group_id: 3cc28c39-f695-45e8-bfe2-411e18bbbce5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of next-auth for Authentication and Session Management: Authentication Configuration Session Provider Initialization Centralized

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for application route handlers, server actions, and library modules managing user authentication or session verification.

### Rules

- **R-AUTH-001** MUST: Authentication configuration and session provider initialization MUST be centralized within a dedicated authentication library module rather than instantiated across individual route handlers.

### Verify

```bash
manifest=$(find . -maxdepth 3 -name "*manifest*" -o -name "*package*" | head -n 1); [ -n "$manifest" ] && grep -q "next-auth" "$manifest" && echo "next-auth dependency verified in manifest"
grep -rn "next-auth" . --exclude-dir=".*" 2>/dev/null | grep -q "next-auth" && echo "next-auth usage present"
sh -c 'run_tests=$(find . -maxdepth 2 -name "*test*" -o -name "*spec*" | head -n 1); [ -n "$run_tests" ] && echo "Run discovered project test runner on authentication test suites"'
```

**Accept when:**
- The project dependency manifest and lock artifact both resolve next-auth to an active, locked version.
- Authentication routes and server actions route session validation through the centralized authentication module.
- Automated test suites discovered in the repository pass without authentication or type-check regressions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>