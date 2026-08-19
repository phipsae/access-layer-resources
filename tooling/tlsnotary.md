---
name: tlsnotary
last_reviewed: 2026-08-19
maturity: usable
implements: [tls-oracles]
guarantees: [O-3, O-4]
---

# TLSNotary

## What it implements

MPC-flavor [tls-oracles](../primitives/tls-oracles.md): the client and a notary jointly run the TLS connection, so neither can forge a transcript alone, and anyone can run the notary. TLS 1.2 only today.

## Integration guide

Prerequisite: a notary, self-hosted or third-party.

1. [app] Use the Rust crates or the browser extension to run the notarized session.
2. [user] Selects what to disclose; the rest of the transcript stays committed but hidden.
3. [app] Verifies the attestation against the notary's key.

The [documentation](https://tlsnotary.org/docs/intro) is the guide.

## Audit and maturity status

Working Rust implementation and browser extension; the self-hostable notary is the differentiator. MIT/Apache-2.0.

## Maintainer

PSE, at [tlsnotary/tlsn](https://github.com/tlsnotary/tlsn).
