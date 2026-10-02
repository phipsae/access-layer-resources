---
name: shielded-pools
last_reviewed: 2026-10-02
maturity: production
guarantees: [D-2, P-1]

related:
  requires: []
  composes_with: [stealth-addresses]
  alternative_to: []
  see_also: [mixnets]
---

# Shielded pools

## What it is

A shielded pool is a contract where deposits join a common anonymity set. Inside the pool, ownership lives in cryptographic commitments spent by nullifiers, so moving or withdrawing funds means proving ownership in zero knowledge, never pointing back at the deposit. Two designs exist: fixed-denomination mixers, which only break the deposit-withdrawal link, and shielded UTXO systems, which support arbitrary amounts and internal transfers with balances hidden throughout.

## What it guarantees

D-2: positions, balances, and history leave the public ledger while funds stay in the pool, as an explicit layer the user opts into. P-1's amount half: stealth addresses hide who receives, shielded pools hide how much and what came before, in the UTXO design; fixed-denomination mixers hide only the link.

## How it works

1. [user] Deposits into the pool; a commitment to the note joins a Merkle tree.
2. [user] Holds the note (value, secret) off chain.
3. [user] Transfers inside the pool by generating a zero-knowledge proof, client-side, that they own an unspent note under the current Merkle root, emitting new commitments with amounts hidden.
4. [contract] Verifies each proof on chain and accepts each spend's nullifier once, which blocks double-spends without revealing which note died.
5. [user] Withdraws with the same kind of proof: membership of some unspent note, without revealing which deposit it came from.

## Trust model

The core needs no operator: circuit soundness is the cryptographic base, and the systems deployed today all run Groth16 with circuit-specific ceremonies, so a live trusted setup is part of it. Relayers that pay withdrawal gas see timing and destinations, and can refuse service; recent roots ([EIP-8272](https://eips.ethereum.org/EIPS/eip-8272)) and keyed nonces ([EIP-8250](https://eips.ethereum.org/EIPS/eip-8250)) for frame transactions, both considered for inclusion in Hegotá, will let spends enter the public mempool without relayers.

## Known limits

- The anonymity set is the binding limit: private traffic is well under 1% of Ethereum activity, and a small crowd hides poorly.
- Timing and amount correlation shrink the effective set further; the funding-link problem applies at entry and exit.
- Regulatory history shapes integration: the earliest pool was sanctioned in 2022 and delisted in 2025, and screening designs exist to keep that from recurring.
- Scaling costs grow with use: nullifier sets and commitment trees only ever grow, and wallets must scan published notes to find their own, a client-side cost that rises with pool activity.
- Post-quantum exposure: a CRQC forges today's pairing-based proofs, enabling undetectable minting inside the pool; post-quantum proof systems with in-mempool aggregation ([EIP-8288](https://eips.ethereum.org/EIPS/eip-8288), Draft) are the route to replacing them. Commitments and nullifiers are hashes and survive. Pools whose in-pool transactions publish note ciphertexts under classical key agreement expose them to harvest-now-decrypt-later attacks.

## Implementations

[Railgun](https://github.com/Railgun-Privacy/contract) (shielded UTXO, mainnet since 2021, deposit screening via Private Proofs of Innocence) and [Privacy Pools](https://github.com/0xbow-io/privacy-pools-core) (association sets, mainnet since 2025). Both are exposed as Kohaku wallet plugins. Tooling card: [kohaku](../tooling/kohaku.md).

## Further reading

- [Zerocash: decentralized anonymous payments from Bitcoin](https://eprint.iacr.org/2014/349), Ben-Sasson et al., 2014; the design ancestor
- [Blockchain privacy and regulatory compliance: towards a practical equilibrium](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4563364), Buterin, Illum, Nadler, Schär, Soleimani, 2023; the Privacy Pools paper
- [Railgun documentation](https://docs.railgun.org/)
- [Private Proofs of Innocence](https://docs.railgun.org/wiki/assurance/private-proofs-of-innocence), Railgun wiki
- [Privacy Pools](https://privacypools.com/), 0xbow
- [EIP-8288: in-mempool signature and proof aggregation](https://eips.ethereum.org/EIPS/eip-8288) (Draft)
- [Towards native post-quantum private ETH](https://ethresear.ch/t/towards-native-post-quantum-private-eth/25291), Pierre, 2026
