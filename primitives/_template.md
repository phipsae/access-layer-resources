---
name: <primitive-slug>
last_reviewed: YYYY-MM-DD

# Maturity of the primitive itself:
#   research   - papers only, no usable implementation
#   concept    - spec exists, no code
#   testnet    - PoC or pilot implementations
#   production - audited implementations in sustained mainnet use
maturity: research | concept | testnet | production

# Guarantee IDs this primitive can satisfy, defined in domains/.
guarantees: [W-1]

# When to reach for this primitive, and when not to. 1-3 bullets each.
works-best-when:
  - <condition>
avoid-when:
  - <condition>

# CROPS profile: badge plus narrative. The badge is the typical rating; the
# narrative says where it drifts up or down in practice.
crops_profile:
  cr: high | medium | low | none
  o: yes | partial | no
  p: full | partial | none
  s: high | medium | low
crops_context:
  cr: "<where the rating drifts, e.g. high with permissionless relays, low behind an operator-run gateway>"
  o: "..."
  p: "..."
  s: "..."

# Post-quantum exposure. Encrypted data recorded today can be harvested now
# and decrypted later (HNDL), so every privacy primitive declares this.
post_quantum:
  risk: high | medium | low
  vector: "<what breaks under a CRQC>"
  mitigation: "<hash-based / lattice-based alternative, migration path>"

# Typed cross-references to other primitives. Slugs match
# primitives/<slug>.md and must resolve to an existing file.
related:
  requires: []        # hard dependency, cannot work without it
  composes_with: []   # works well together, optional
  alternative_to: []  # competing approach to the same guarantee
  see_also: []        # related for context

# OPTIONAL, for primitives with materially different variants (trust models
# that diverge, e.g. a shared-state primitive with co-SNARK / FHE / TEE
# flavors). Give each variant its own file and list the slugs here; keep this
# file as the aggregator. Delete otherwise.
# variants:
#   - name: "<variant name>"
#     primitive: <variant-slug>
#     summary: "<one-line trust-model difference>"
---

# <Primitive name>

## What it is

First-principles explanation in a few paragraphs. No project names here; implementations belong in [tooling/](../tooling/).

## What it guarantees

The guarantee(s) this primitive provides, referencing the IDs in frontmatter, and the mechanism by which it provides them.

## How it works

Numbered steps (5-8 max), each prefixed with the actor performing it in square brackets: `[user]`, `[wallet]`, `[contract]`, `[relayer]`, `[node]`, `[operator]`.

1. [user] ...
2. [wallet] ...

## Trust model

Who or what must behave honestly for the guarantee to hold, and what breaks when they fail. Honest-majority assumptions, hardware assumptions, liveness assumptions.

## Known limits

Performance costs, anonymity-set considerations, UX costs, and other limits a builder should know before committing. CROPS drift and post-quantum exposure live in frontmatter, not here.

## Implementations

Links to [tooling/](../tooling/) cards.

## Further reading

Papers and specs, as pinned permalinks.
