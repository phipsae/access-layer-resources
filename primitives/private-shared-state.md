---
name: private-shared-state
last_reviewed: 2026-08-27
maturity: usable
guarantees: [D-1, D-2]

related:
  requires: []
  composes_with: [shielded-pools, validity-proofs]
  alternative_to: []
  see_also: [encrypted-mempools, trusted-execution-environments, private-rollups]
---

# Private shared state

## What it is

Private shared state is application state that no single party can read while multiple parties transact against it: a dark-pool orderbook that matches orders nobody sees, a sealed-bid auction, a lending book with hidden positions. The defining property is jointness. Personal-state systems ([shielded-pools](shielded-pools.md), client-side proving in [private-rollups](private-rollups.md)) hide one user's state from everyone else, and the user holds the plaintext. Here the plaintext belongs to no one: computation runs over encrypted or secret-shared state that stays hidden from the participants themselves, the operator included.

Four constructions carry it, differing in who or what replaces the trusted operator: secure multi-party computation, fully homomorphic encryption, hardware enclaves, and collaborative SNARKs.

## What it guarantees

D-1: an order entering a private matching engine is unreadable between submission and execution, so nothing upstream can act on it first. D-2: positions held inside the shared state publish no balances, strategies, or liquidation levels, as an explicit layer on a transparent L1. Settlement returns to the public chain, usually carried by [validity-proofs](validity-proofs.md) that the hidden computation followed the rules.

## How it works

The MPC dark-pool flow, the shape deployed today:

1. [user] Secret-shares an order into the matching network; no node holds the plaintext.
2. [operator] Matching nodes jointly compute over the shares, learning only whether a match exists.
3. [user] On a match, the counterparties cooperatively generate a proof that the fill follows the stated rules.
4. [contract] Verifies the proof and settles the net result on chain.
5. [user] Unmatched order shares expire or withdraw without ever having been readable.

## Trust model

Each construction substitutes a different assumption for the trusted operator. MPC: privacy holds while the computing parties do not collude past the threshold. FHE: computation is encrypted under a network key, and a threshold committee holding the decryption shares must not collude; the cryptography carries the compute, the committee carries the key. Enclaves: hardware isolation, with the trust profile of [trusted-execution-environments](trusted-execution-environments.md). Collaborative SNARKs: MPC nodes jointly produce a validity proof, inheriting MPC's non-collusion assumption for privacy and the proof system's soundness for correctness. In every flavor, correctness and privacy separate: settlement proofs keep the state transitions honest even where the privacy assumption fails.

## Known limits

- Latency and cost are structural: MPC pays network rounds per operation, FHE pays orders of magnitude in compute, and both bound throughput well below plain contract execution.
- Threshold key custody is the FHE flavor's standing exposure: the committee that can decrypt is a standing target for both attack and legal compulsion.
- Each private venue fragments liquidity and its anonymity set; a venue with few participants hides little and matches less.
- Composability narrows: public contracts cannot read the hidden state, so integrations pass through explicit encrypted interfaces.
- Post-quantum exposure is split: FHE is lattice-based and holds, secret sharing is information-theoretic and holds, while the pairing-based settlement proofs deployed today are forgeable by a CRQC, the same soundness exposure shielded pools carry.

## Implementations

[Renegade](https://github.com/renegade-fi/renegade) (MPC order matching with collaborative SNARK settlement, live on Arbitrum since 2024) and [Zama's fhEVM](https://github.com/zama-ai/fhevm) (FHE contracts under threshold decryption, Ethereum mainnet since late 2025). Tooling cards pending.

## Further reading

- [Zama protocol litepaper](https://docs.zama.org/protocol/zama-protocol-litepaper)
- [Experimenting with collaborative zk-SNARKs](https://eprint.iacr.org/2021/1530), Ozdemir and Boneh, 2021
