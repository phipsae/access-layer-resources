---
name: <tool-slug>
last_reviewed: YYYY-MM-DD

# Maturity of this implementation (not of the underlying primitive):
maturity: research | concept | usable | production

# The primitive this tool implements. Slug matches primitives/<slug>.md.
implements: <primitive-slug>

# Guarantee IDs this tool satisfies when integrated as documented.
guarantees: [W-1, W-3]

works-best-when:
  - <condition>
avoid-when:
  - <condition>
---

# <Tool name>

<!-- TODO: scope to be tightened before the first real card. Open questions:
does tooling/ cover apps, open source libraries, open source tools, or all
three? How do we treat an open source tool that also has a paid plan? -->

## What it implements

The [primitive](../primitives/) this tool implements, and how faithfully: any deviations from the primitive's ideal trust model (hosted components, default endpoints, telemetry) belong here, stated plainly.

## Integration guide

The shortest credible path from zero to the guarantee holding in production: prerequisites, steps, and a minimal example. Steps carry actor tags where multiple parties are involved: `[app]`, `[user]`, `[contract]`, `[relayer]`.

This section is the reason the repository exists; keep it current.

## Audit and maturity status

Audit reports (linked), production usage, maintenance status. Facts only, no endorsement.

## Maintainer

Who builds and maintains this, with links to the repository and documentation.
