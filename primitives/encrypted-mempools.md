---
name: encrypted-mempools
last_reviewed: 2026-08-19
maturity: usable
guarantees: [D-1, R-1]

related:
  requires: []
  composes_with: [mixnets]
  alternative_to: []
  see_also: []
---

# Encrypted mempools

## What it is

An encrypted mempool keeps transactions unreadable until their ordering is fixed, so nobody in the transaction supply chain can act on intent before execution. The primitive is the commit-then-reveal ordering discipline; what varies is the decryption trigger: a threshold committee releasing key shares, delay encryption that opens by elapsed time, or hardware enclaves releasing plaintext at inclusion. The threshold and enclave flavors are deployed today on a sidechain and an L2; Ethereum L1 has no encrypted mempool.

## What it guarantees

D-1: the read that makes sandwiching and front-running possible no longer exists before ordering commits. Extraction that needed the payload is removed; extraction that only needs statistics survives. The same blindness serves censorship resistance (R-1): an orderer cannot selectively exclude what it cannot read, so content-based filtering fails until the ordering is already fixed.

## How it works

1. [user] Encrypts the transaction so its content is unreadable at submission, bound to a commitment that ties it to eventual inclusion.
2. [node] Orders ciphertexts and commits the order, without reading any payload.
3. [operator] The scheme's trigger fires: enough committee shares released, the time lock elapsing, or an enclave releasing plaintext.
4. [node] Transactions decrypt and execute in the pre-committed order.
5. [node] Payloads that fail to decrypt or turn out invalid are handled per scheme, skipped or penalized.

## Trust model

Threshold schemes trust a t-of-n keyper committee twice: for privacy, since collusion above the threshold decrypts early and silently, returning MEV without leaving evidence; and for liveness, since too few honest shares stall decryption. Delay encryption trades the committee for timing assumptions and sequential-computation hardware; enclaves trade it for the hardware vendor. In the threshold and delay flavors the ordering commitment, once made, is publicly checkable; in the enclave flavor checking reduces to trusting the attestation.

## Known limits

- Latency: a transaction cannot execute before its decryption trigger, and the delay is per transaction, every time.
- Metadata still leaks: ciphertext size, sender, and gas payment stay visible, and statistical front-running on those signals survives encryption.
- Deployments are opt-in with thin adoption, so the protected set is small and self-selected.
- On Ethereum L1, enshrinement is a draft proposal; nothing is close to inclusion.
- Post-quantum exposure is bounded to the pre-inclusion window for included transactions; ciphertexts that never decrypt (dropped, invalid, or cancelled) stay published and remain harvestable while threshold keys rest on classical curves.

## Implementations

[Shutter](https://github.com/shutter-network/rolling-shutter) (threshold committee; opt-in on Gnosis Chain since 2024) and [Rollup-Boost](https://github.com/flashbots/rollup-boost) (enclave-based; live on Unichain since 2025), with an Ethereum path through the PBS pipeline via mev-commit preconfirmations. Tooling cards pending.

## Further reading

- [The road towards an encrypted mempool on Ethereum](https://docs.shutter.network/docs/shutter/research/the_road_towards_an_encrypted_mempool_on_ethereum), Shutter research
- [The first encrypted mempool is coming to PBS on Ethereum](https://blog.shutter.network/the-first-encrypted-mempool-is-coming-to-pbs-on-ethereum/), Shutter and Primev
- [Introducing the universal enshrined encrypted mempool EIP](https://blog.shutter.network/introducing-the-universal-enshrined-encrypted-mempool-eip/), EIP-8105 background
- [Threshold encrypted mempools with mev-commit preconfirmations](https://ethresear.ch/t/threshold-encrypted-mempools-with-mev-commit-preconfirmations/23588), ethresear.ch
