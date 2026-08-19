---
name: rollup
last_reviewed: 2026-08-17
---

# Rollup

Rollups execute transactions off Ethereum L1 and settle back to it. The domain has two kinds of user: applications that deploy on a rollup and end users transacting on it, and both depend on the same two pressure points, the sequencer as a chokepoint and the exit path back to L1, which must stay usable without the operator's cooperation. Bridging to and from other chains belongs here as an exit and interop function.

## Functions

- Sequencing (ordering and inclusion)
- Execution and proving
- Data availability
- Exit, withdrawal, and bridging
- Upgrade governance

## Guarantees

### R-1: Inclusion without the sequencer [CR]

- **Property**: A user can get a transaction included and executed without the sequencer's cooperation, within a bounded delay, at a cost that keeps the right practical.
- **Functions**: [Sequencing]
- **Motivation**: The top rollups by value all run a single sequencer today. Where the proof system is live it cannot steal, but it can refuse and reorder, and a refused transaction is a frozen account for as long as the refusal lasts; with a liquidation pending, the delay is the loss.
- **Primitives**: [based-sequencing](../primitives/based-sequencing.md); [decentralized-sequencing](../primitives/decentralized-sequencing.md); [encrypted-mempools](../primitives/encrypted-mempools.md), which blind content-based filtering; L1 force-inclusion queues are protocol design, prose only.
- **Upgrade path**: Force-inclusion queues exist on most major rollups, with delay windows from half a day to a day, and at least one major rollup ships none at all; the cost and timing limits of the forced path are the differentiator. Based sequencing inherits L1's inclusion properties for hard inclusion, with preconfirmations reintroducing a weaker trust assumption for fast UX. Decentralized sequencer sets remove the single operator, and the first permissionless set is live.

### R-2: Exit without the operator [CR, S]

- **Property**: A user can withdraw assets to L1 with nothing but L1 access and public data, even if the operator vanishes; any upgrade able to break this opens an exit window long enough to leave first.
- **Functions**: [Exit, withdrawal, and bridging; Upgrade governance]
- **Motivation**: This is the walkaway test applied to L2s: an L2 balance is an IOU against the operator's continued existence unless exit is self-service. Third-party bridges do not substitute; they reintroduce the custodial chokepoint the native exit removes.
- **Primitives**: None carded; escape hatches and forced withdrawals are protocol design.
- **Upgrade path**: The L2Beat Stages framework is the shared yardstick: Stage 1 requires a functional proof system and exits that survive the operator, with a security council as backstop; Stage 2 adds an exit window of at least 30 days and restricts intervention to bugs provable on chain. Rollups have reached Stage 2 both by shipping immutable contracts from day one and, most recently, by revoking ownership of upgradeable ones.

### R-3: Trustless state updates [S]

- **Property**: No operator's word defines who owns what: state transitions are enforced by a live proof system (validity or fraud proofs) that is permissionless to operate.
- **Functions**: [Execution and proving]
- **Motivation**: Without a functional proof system, whoever posts state roots decides every balance, and the chain is a multisig with a website. R-1 and R-2 then become promises against that same word.
- **Primitives**: [validity-proofs](../primitives/validity-proofs.md); [fraud-proofs](../primitives/fraud-proofs.md).
- **Upgrade path**: A functional proof system is the Stage 1 gate; permissionless challenge submission is the Stage 2 gate for fraud-proof systems, and validity rollups clear it through forced exits. The large optimistic rollups have opened proving permissionlessly; the major validity rollups still run centralized provers, and that is the trust that remains.

### R-4: Data availability for permissionless reconstruction [CR, S]

- **Property**: Everything needed to reconstruct state and exercise an exit is published on L1, or on a DA layer whose weaker guarantees the rollup states plainly.
- **Functions**: [Data availability]
- **Motivation**: Proofs and escape hatches are theater if the data to use them is withheld. Data availability is what turns R-1 through R-3 from rights on paper into rights a user can exercise alone.
- **Primitives**: [data-availability-sampling](../primitives/data-availability-sampling.md).
- **Upgrade path**: Publishing to L1 blobs is the default that keeps reconstruction permissionless. Data-availability sampling, live on L1 since late 2025, keeps raising how much it can carry. External DA committees trade reconstruction away for cost, and that tradeoff belongs in the open.

### R-5: Private execution as a rollup-level option [P]

- **Property**: A rollup can make execution and state private by default (client-side proving, encrypted or committed state) while still settling to and exiting through Ethereum; where it does, R-2 through R-4 hold for the private state too.
- **Functions**: [Execution and proving, Data availability]
- **Motivation**: Privacy retrofitted app by app inherits each app's anonymity-set problem. A rollup that is private by default gives every application on it the same cover, and settlement on Ethereum keeps the exit and validity guarantees that privacy usually costs.
- **Primitives**: [private-rollups](../primitives/private-rollups.md).
- **Upgrade path**: One natively private rollup is live on mainnet, in alpha, at Stage 2 with a permissionless sequencer set. The open questions are client-side proving cost and how private state interacts with forced exits.

## Ship it

No integration guides yet; tooling cards pending.
