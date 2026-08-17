---
name: <tool-slug>
last_reviewed: YYYY-MM-DD

# Maturity of this implementation (not of the underlying primitive):
maturity: research | concept | usable | production

# Optional: primitives this tool gives an app access to. Slugs match
# primitives/<slug>.md and must resolve. Omit when no card applies.
implements: [<primitive-slug>]

# Guarantee IDs this tool satisfies when integrated as documented.
guarantees: [W-1, W-3]
---

# <Tool name>

<!-- Scope (decided 2026-08-17): tooling/ covers open source software an
application can integrate: libraries, SDKs, reference implementations, and
self-hostable services. End-user apps get cards only for their integrable
parts. A tool with a paid hosted plan qualifies if the documented integration
path is fully open source and self-hostable; the card states this plainly. -->

## What it implements

The [primitive](../primitives/) this tool implements, and how faithfully: any deviations from the primitive's ideal trust model (hosted components, default endpoints, telemetry) belong here, stated plainly.

## Integration guide

The shortest credible path from zero to the guarantee holding in production: prerequisites, steps, and a minimal example. Steps carry actor tags where multiple parties are involved: `[app]`, `[user]`, `[contract]`, `[relayer]`.

This section is the reason the repository exists; keep it current.

## Audit and maturity status

Audit reports (linked), production usage, maintenance status. Facts only, no endorsement.

## Maintainer

Who builds and maintains this, with links to the repository and documentation.
