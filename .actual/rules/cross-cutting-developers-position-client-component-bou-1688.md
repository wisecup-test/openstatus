---
version: 2
name: cross-cutting-developers-position-client-component-bou-1688
description: "Client Component Directive (\"use client\") in @openstatus/ui Component Library: Developers Position Client Component Boundaries Close"
metadata:
  actual:
    adr_id: 1688b1f8-d8f9-4f45-b026-919ed8ada7b3
    status: accepted
    candidate_group_id: 4802f371-80d7-4d9d-91c4-2dfa24bf57b8
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
---

# Client Component Directive ("use client") in @openstatus/ui Component Library: Developers Position Client Component Boundaries Close

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLIENT-001** SHOULD: Position client component boundaries as close to the leaves of the component render tree as possible to maximize server-side rendering opportunities for non-interactive parent structures.
- **R-CLIENT-002** MANDATORY: Place the client component directive as the very first statement in the module, preceding all import statements and comments.
- **R-CLIENT-003** MANDATORY: When composing composite blocks from multiple UI primitives, ensure that any child component requiring DOM event handlers or state hooks is enclosed within a client boundary.

### Verify

```bash
# Discover and run repository lint and check scripts from project manifests
MANIFEST_SCRIPTS=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"lint"' {} + 2>/dev/null | head -n 1)
echo "Discovered lint configuration in: $MANIFEST_SCRIPTS"

# Discover component files importing interactive hooks and verify client boundary declarations
find . -type f -name '*.*' -exec grep -lE 'useState|useEffect|useMemo' {} + | while read -r comp; do head -n 5 "$comp" | grep -q '"use client"' || echo "Verify client boundary in: $comp"; done

# Discover and run repository build validation scripts to confirm rendering model integrity
MANIFEST_BUILD=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"build"' {} + 2>/dev/null | head -n 1)
echo "Discovered build configuration in: $MANIFEST_BUILD"
```

**Accept when:**
- All UI component modules utilizing client-side hooks or browser event listeners declare the client boundary directive as their initial statement.
- Repository build and lint verification scripts complete with zero exit code and no client-server boundary violation errors.
- Component rendering in both server-side and client-side execution environments completes without boundary mismatch warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>