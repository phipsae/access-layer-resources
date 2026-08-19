---
name: oracle
last_reviewed: 2026-08-17
---

# Oracle

Oracles bring off-chain facts on chain: prices, events, randomness. Everything downstream inherits their trust model, so whoever operates or aggregates a feed can distort it, censor it, or act on it first.

## Functions

- Data sourcing
- Aggregation and reporting
- On-chain delivery and updates
- Randomness
- User-data attestation

## Guarantees

### O-1: Manipulation-resistant values [S]

- **Property**: No single reporter, source, or thin market can move the reported value. Aggregation spans independent sources and reporters, and deviation from the broad market is bounded and detectable.
- **Functions**: [Data sourcing, Aggregation and reporting]
- **Motivation**: Everything downstream executes against the feed. Manipulated price feeds have drained lending protocols without touching a single contract bug, because the feed was the bug.
- **Primitives**: None carded; median aggregation across independent reporters and volume-weighted sourcing are design practices.
- **Upgrade path**: Aggregate across independent reporters and sources rather than venues that share liquidity. Bound accepted deviation per update. Prefer market breadth over update speed for values that trigger liquidations.

### O-2: Permissionless reporting and recovery [CR]

- **Property**: Feed operation does not depend on an allowlisted set's continued goodwill: anyone meeting an open, stake-based bar can report, and a consumer can replace or reconstruct a stopped feed without the operator.
- **Functions**: [Aggregation and reporting, On-chain delivery and updates]
- **Motivation**: An allowlisted reporter set is a chokepoint with a schedule. It can delist an asset, pause updates under pressure, or price its continued service, and the consuming app inherits every one of those decisions.
- **Primitives**: None carded; open reporter sets with staking and slashing are protocol design.
- **Upgrade path**: Prefer feeds with open reporter admission. Keep a fallback source wired in with defined switchover behavior, the switch itself behind the delays defi D-4 describes. Design consumers to degrade safely when a feed goes stale.

### O-3: Verifiable provenance [O, S]

- **Property**: A consumer can verify where a value came from and how it was computed: signed source data, provable web sessions, or randomness whose proof verifies on chain. No black-box pipeline sits between source and contract.
- **Functions**: [Data sourcing, Randomness]
- **Motivation**: A feed that says "trust our aggregation" is a privileged specification: the one part of an on-chain application whose correctness nobody outside can check.
- **Primitives**: `tls-oracles` for source data provable without the source's cooperation; `verifiable-randomness` for draws whose proof verifies on chain; [zkvm](../primitives/zkvm.md) for proving the aggregation itself.
- **Upgrade path**: Verifiable randomness is production-standard, with withholding as the residual bias vector. Web proofs are in production for user-facing attestation and entering reporter pipelines; signed exchange data covers the price path where exchanges cooperate. zkVM pipelines that prove the aggregation itself are emerging, with zk-verified aggregation live in early deployments; there, the reported value becomes verifiably the stated function of its signed inputs.

### O-4: Private user-data attestation [P]

- **Property**: When an app needs a fact about a user's off-chain data (a balance, a rating, an account attribute), the user proves the fact from their own session. The oracle operator never holds the user's credentials and never sees more than the proven predicate.
- **Functions**: [User-data attestation]
- **Motivation**: The naive design routes the user's login through the oracle operator, turning one app's need for one fact into a custodian of everyone's accounts. The operator becomes the honeypot and the chokepoint at once.
- **Primitives**: `tls-oracles`; overlaps deliberately with identity's [anonymous-credentials](../primitives/anonymous-credentials.md), since a web proof is a credential whose issuer never signed up to be one.
- **Upgrade path**: Production web-proof stacks run the proving circuit on the user's device, over the user's own TLS session, in seconds, with an attestor participating as a live witness. The trust residues to state are the proxy or notary position in each scheme (forgery on collusion, censorship; never plaintext exposure) and the source site's ability to change formats or block the proof path.

## Ship it

Integration guides by guarantee, added as tooling cards land:

- O-4: web-proof tooling (cards pending)
