---
name: decentralized-sequencing
last_reviewed: 2026-08-19
maturity: usable
guarantees: [R-1]

related:
  requires: []
  composes_with: []
  alternative_to: [based-sequencing]
  see_also: []
---

# Decentralized sequencing

## What it is

A permissionless staked set replaces the rollup's single sequencer: anyone meeting the stake joins, leaders rotate by election, and misbehavior is slashed. The rollup keeps its own fast cadence and fee control, unlike based sequencing, which hands ordering to L1.

## What it guarantees

R-1 by plurality rather than inheritance: no single operator can refuse a transaction, because refusing means every elected leader refusing, and the set is open. Censorship becomes a coordination problem across strangers with stake at risk.

## How it works

1. [operator] Stakes to join the sequencer set, permissionlessly.
2. [node] Leader election assigns slots or epochs across the set, with committees sampled where the set is large.
3. [operator] The leader orders and proposes; attesters from the set confirm.
4. [contract] Misbehavior is slashed, by on-chain proof where provable and by validator vote in the live design; censorship is punished socially rather than proven. Rotation limits any leader's window.

## Trust model

An honest majority of the staked set, and a fair, unbiasable leader election. A leader can still reorder or delay within its own slot; rotation bounds the damage rather than removing it.

## Known limits

- Latency sits above a single sequencer's: consensus among strangers costs what it costs.
- MEV moves inside the leader set instead of disappearing; per-slot leaders inherit per-slot extraction.
- A small or stake-concentrated set recentralizes quietly; the guarantee is only as good as the set's diversity.
- Protocol complexity is real: election, slashing, and committee sampling are consensus engineering, maintained forever.
- Post-quantum: signature schemes for attestation are classical today, the same migration Ethereum's own consensus faces.

## Implementations

[Aztec](https://github.com/AztecProtocol/aztec-packages) runs the first permissionless sequencer set on mainnet (thousands of validators, sampled committees, live since March 2026, network in alpha); a major L2 targets decentralized sequencing in 2026. Tooling cards pending.

## Further reading

- [Decentralised sequencing](https://medium.com/l2beat/decentralised-sequencing-4441edf5852a), L2Beat, 2026
- [Aztec network documentation](https://docs.aztec.network/)
