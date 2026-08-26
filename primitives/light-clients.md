---
name: light-clients
last_reviewed: 2026-08-26
maturity: usable
guarantees: [W-4, P-4, X-2]

related:
  requires: []
  composes_with: [pir, zkvm]
  alternative_to: []
  see_also: [mixnets]
---

# Light clients

## What it is

A light client verifies chain data against Ethereum's consensus commitments with the resources a phone or browser has, instead of trusting whatever an RPC endpoint answers. Since the Altair upgrade, a rotating sync committee of 512 validators signs block headers, so a client can follow the chain by checking one aggregate signature per update it accepts, at minimum one per committee period of about 27 hours, rather than re-executing blocks.

Two styles work today: header-following clients that stay synced and verify responses as they come, and proof-driven stateless clients that keep no state and verify consensus plus execution proofs on demand, per query.

The Glamsterdam fork changes what a header carries and leaves the sync protocol untouched. Under [EIP-7732](https://eips.ethereum.org/EIPS/eip-7732), a verified header commits to a builder's payload bid, revealed mid-slot: confirming execution takes payload status alongside the header, and execution-state proofs for a slot derive only after the reveal. [EIP-7928](https://eips.ethereum.org/EIPS/eip-7928) commits each header to a per-block diff of every state access. A node that applies those diffs without executing tracks state at low cost and verifies nothing locally; it rests on executing validators rejecting blocks whose lists lie, an economic basis outside this card's guarantee.

## What it guarantees

Turns any untrusted endpoint into a verified data source: the endpoint can refuse to answer, but a wrong answer fails verification locally. This is the intermediary-free read path behind W-4, the local settlement confirmation of P-4, and the verification root X-2 anchors to. It does nothing for read privacy: the endpoint still sees which addresses are queried, which is `pir` territory.

## How it works

1. [user] Obtains a weak-subjectivity checkpoint, a block hash known to belong to the canonical chain, from one or more trusted sources.
2. [wallet] Syncs sync-committee updates from the checkpoint forward and verifies each update's aggregate signature.
3. [node] Serves headers, state, and receipts with Merkle proofs on request.
4. [wallet] Checks every response against the verified header's roots.
5. [wallet] Exposes a standard RPC interface locally, now verified instead of trusted.

## Trust model

Three assumptions: the sync-committee sample stays honest (512 validators, a deliberately cheaper bar than full consensus), the initial checkpoint is canonical (a malicious checkpoint syncs the client to the wrong chain, so it deserves cross-checking), and some endpoint stays willing to serve data. Endpoints can withhold; they can never forge.

## Known limits

- Sync-committee security is a sample of consensus; corrupting one committee costs far less than attacking Ethereum.
- Finality lags around fifteen minutes; verified does not mean instant.
- Verification covers state and inclusion; completeness of derived histories needs `zkvm`-class proofs on top.
- Reference clients remain unaudited and are provided as-is; treat primary-RPC use for high-value flows accordingly.
- Post-quantum exposure sits in the signatures: sync-committee BLS is forgeable by a CRQC, and hash-based signature schemes are the migration path under research.

## Implementations

[Helios](https://github.com/a16z/helios) (header-following, multichain, ~2s sync) and [Colibri](https://github.com/corpus-core/colibri-stateless) (proof-driven, stateless); both are packaged in the Kohaku provider layer. Tooling cards pending.

## Further reading

- [Altair light-client sync protocol](https://github.com/ethereum/consensus-specs/blob/master/specs/altair/light-client/sync-protocol.md) (consensus-specs)
- [An introduction to light clients](https://a16zcrypto.com/posts/article/an-introduction-to-light-clients/), a16z crypto
- [Building Helios](https://a16zcrypto.com/posts/article/building-helios-ethereum-light-client/), a16z crypto
- [Proof of stake: how I learned to love weak subjectivity](https://blog.ethereum.org/2014/11/25/proof-stake-learned-love-weak-subjectivity), Vitalik Buterin, 2014
- [Light clients](https://ethereum.org/en/developers/docs/nodes-and-clients/light-clients/), ethereum.org
