---
name: validity-proofs
last_reviewed: 2026-08-19
maturity: production
guarantees: [R-3]

related:
  requires: []
  composes_with: [zkvm]
  alternative_to: [fraud-proofs]
  see_also: []
---

# Validity proofs

## What it is

A validity rollup ships every state transition with a cryptographic proof that L1 verifies before accepting the new state root. Wrong states cannot land, ever; there is no challenge window because there is nothing to challenge.

## What it guarantees

R-3 in its strongest form: no operator's word defines balances, because the word never enters the system. What the L1 contract accepted is what the proven program computed, and finality toward L1 is as fast as proving allows.

## How it works

1. [operator] Batches transactions and executes them off chain.
2. [operator] Generates a proof of the state transition, from hand-built circuits or a general-purpose zkVM.
3. [contract] The L1 verifier checks the proof against the old root, the new root, and the published data.
4. [contract] The new root is final on acceptance; exits can proceed against it immediately.

## Trust model

The same two layers as any proof system: the cryptography's soundness and the circuits' correctness, a bug in either meaning false states verify. The prover is a liveness chokepoint, never a safety one: a stalled prover stalls state roots and therefore exits, but cannot forge them.

## Known limits

- Proving costs real money and time per batch; the latency budget shapes throughput.
- The largest deployments still run centralized or allowlisted provers, with one live exception already proving permissionlessly; for the rest, permissionless proving is the maturity gate that remains ahead.
- Trusted setups apply where the proof system needs one.
- Post-quantum: pairing-based verifiers are CRQC-forgeable and need swapping; STARK-based stacks largely survive. The swap changes the verifier, not the rollup's programs.

## Implementations

Rollup proving stacks in sustained production use include [zksync-era](https://github.com/matter-labs/zksync-era) and [Stwo](https://github.com/starkware-libs/stwo) (Starknet's prover since late 2025); general-purpose zkVMs (see the [zkvm](zkvm.md) card) increasingly carry the same role. Tooling cards pending.

## Further reading

- [ZK-rollups](https://ethereum.org/en/developers/docs/scaling/zk-rollups/), ethereum.org
- [An incomplete guide to rollups](https://vitalik.eth.limo/general/2021/01/05/rollup.html), Vitalik Buterin, 2021
- [PLONK](https://eprint.iacr.org/2019/953), Gabizon, Williamson, Ciobotaru, 2019
