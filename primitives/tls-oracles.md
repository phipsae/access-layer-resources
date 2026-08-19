---
name: tls-oracles
last_reviewed: 2026-08-19
maturity: production
guarantees: [O-3, O-4]

related:
  requires: []
  composes_with: [anonymous-credentials]
  alternative_to: []
  see_also: [zkvm]
---

# TLS oracles

## What it is

A TLS oracle proves facts from an ordinary HTTPS session to a third party: a balance, a rating, a payment confirmation, from any website, without the site cooperating or even knowing. Two schemes carry it. In the proxy model an attestor relays the encrypted traffic and signs what it observed, and the client proves the content in zero knowledge. In MPC-TLS the client and a notary jointly run the TLS connection itself, so neither can forge a transcript alone.

## What it guarantees

O-3: source data becomes provable without the source's cooperation, which opens the long tail of web data no exchange or API signs. O-4: the user proves the fact from their own session, so no oracle operator ever holds their credentials or sees more than the proven predicate.

## How it works

1. [user] Opens the TLS session to the source through the scheme's attestor path.
2. [operator] The attestor observes or co-computes only ciphertext and commits to the transcript.
3. [user] Proves the predicate over the transcript in zero knowledge, client-side.
4. [app] Verifies the attestor's signature and the proof; on-chain verifiers exist for contract consumption.

## Trust model

The attestor can forge only in collusion with the prover, can censor by refusing service, and never reads plaintext. The source site holds soft power: it can change formats, block attestor addresses, or adopt TLS features a scheme does not cover. Multi-attestor setups dilute the single attestor's position.

## Known limits

- Protocol coverage bounds the MPC flavor: TLS 1.2 only today, with 1.3 on the roadmap.
- Per-source templates are maintenance: a site redesign silently breaks its proofs.
- The attestor is a liveness dependency even where it is privacy-blind.
- Post-quantum: attestor signatures are classical, so a CRQC forges attestations. Recorded TLS 1.2 sessions, and 1.3 sessions without hybrid post-quantum key exchange, are harvestable for later decryption; hybrid key exchange is already the browser default and covers a growing share of the web.

## Implementations

[Reclaim](https://github.com/reclaimprotocol) (proxy model, mobile proving in seconds), [TLSNotary](https://github.com/tlsnotary/tlsn) (MPC-TLS), [zkp2p, now Peer](https://github.com/zkp2p) (web-proof on/off-ramps in production). Tooling cards pending.

## Further reading

- [DECO: liberating web data using decentralized oracles for TLS](https://arxiv.org/abs/1909.00938), Zhang et al., 2019; the academic ancestor
- [TLSNotary documentation](https://tlsnotary.org/docs/intro)
- [Proxying is enough](https://blog.reclaimprotocol.org/posts/proxying-is-enough), Reclaim; the formal case for the proxy model
- [Peer documentation](https://docs.peer.xyz/) (formerly zkp2p)
