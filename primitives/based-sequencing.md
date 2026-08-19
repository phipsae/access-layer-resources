---
name: based-sequencing
last_reviewed: 2026-08-19
maturity: production
guarantees: [R-1]

related:
  requires: []
  composes_with: []
  alternative_to: [decentralized-sequencing]
  see_also: [encrypted-mempools]
---

# Based sequencing

## What it is

A based rollup lets Ethereum's own block proposers sequence it: whoever proposes the next L1 block can include the rollup's next batch. There is no separate sequencer to operate, censor, or decentralize; ordering is a byproduct of L1 block production.

## What it guarantees

R-1 by inheritance: inclusion and liveness are L1's, so getting a transaction in without anyone's cooperation costs exactly what an L1 transaction costs, and censoring the rollup means censoring Ethereum. The chokepoint the guarantee targets never gets built.

## How it works

1. [user] Submits a rollup transaction; it propagates like any L1-visible payload.
2. [node] Any L1 proposer (or a searcher-builder acting for one) assembles the next rollup batch, permissionlessly.
3. [contract] The rollup's inbox on L1 accepts the batch in the L1 block; the rollup's order is derived from L1's order.
4. [node] Rollup nodes execute the derived sequence; proving proceeds as in any rollup.

## Trust model

L1 consensus is the whole trust base for ordering and inclusion. Preconfirmations, added for fast UX because L1 cadence is twelve seconds, reintroduce a smaller trusted set: whoever issues them can break a promise that L1 never made.

## Known limits

- Confirmation cadence is L1's without preconfirmations; interactive UX needs them and pays their trust cost.
- Every inclusion pays L1 gas; cheap sub-second batching is what a dedicated sequencer bought, and it is what this design gives up.
- MEV migrates to L1 proposers and builders rather than disappearing.
- The rollup forgoes sequencer MEV revenue, keeping only base fees, which reshapes its economics.
- Post-quantum: nothing specific to the design; it inherits L1's exposure and migrations.

## Implementations

[Taiko](https://github.com/taikoxyz/taiko-mono), based on mainnet since May 2024, with preconfirmations layered for UX. Tooling cards pending.

## Further reading

- [Based rollups: superpowers from L1 sequencing](https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016), Justin Drake, 2023
- [Taiko documentation](https://docs.taiko.xyz/)
- [Forced transactions vs based sequencing](https://scalability.guide/posts/forced_vs_based/), scalability.guide
