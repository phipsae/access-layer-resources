---
name: private-rollups
last_reviewed: 2026-08-19
maturity: usable
guarantees: [R-5]

related:
  requires: [validity-proofs]
  composes_with: []
  alternative_to: []
  see_also: [shielded-pools]
---

# Private rollups

## What it is

A private rollup makes execution and state private by default: users execute their transactions locally and prove them correct client-side, the network sees only commitments and nullifiers, and the whole thing settles to Ethereum like any validity rollup. Privacy is the chain's baseline rather than an application bolted on top.

## What it guarantees

R-5: every application on the rollup shares one anonymity set, the network's, instead of each app assembling its own crowd. Settlement on Ethereum keeps the exit and validity guarantees that privacy systems usually trade away, and privacy holds against the sequencer itself.

## How it works

1. [user] Executes the private function locally, over notes only they can decrypt.
2. [user] Generates a client-side proof that the state transition is valid.
3. [operator] The sequencer orders proofs it cannot read and aggregates them.
4. [operator] The rollup's own validity proof wraps the batch.
5. [contract] Ethereum verifies and settles; exits flow through the standard path.

## Trust model

The standard validity-rollup stack (sequencing, data availability, proof soundness) plus nothing new for privacy: the client-side proof means no party, sequencer included, ever holds the plaintext. What the user trusts is their own device and the circuits.

## Known limits

- Client-side proving cost is the UX bill, seconds to tens of seconds per interaction, and it shapes what applications feel viable.
- Private state versus forced exits is open design: an escape hatch for notes nobody else can read is harder than one for public balances.
- The one live network is in alpha, with disclosed vulnerabilities and audits ongoing.
- The anonymity set is network usage; a quiet private chain protects little.
- Public-private composability has friction: crossing the boundary leaks timing and amounts unless handled deliberately.
- Post-quantum: inherits its proof system's exposure, per the validity-proofs card.

## Implementations

[Aztec](https://github.com/AztecProtocol/aztec-packages): private smart contracts on mainnet since March 2026 (alpha), L2Beat Stage 2, permissionless sequencer set. Tooling cards pending.

## Further reading

- [Aztec documentation](https://docs.aztec.network/)
- [Announcing the Alpha network](https://aztec.network/blog/announcing-the-alpha-network), Aztec
- [PLONK](https://eprint.iacr.org/2019/953), the proof-system lineage Aztec contributed
