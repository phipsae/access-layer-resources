---
name: anon-rpc
last_reviewed: 2026-08-19
maturity: usable
guarantees: [W-1]
---

# anon-rpc

## What it implements

A standard, with a reference implementation, for anonymized RPC from a browser: a hash-pinned worker runs in a null-origin sandbox with a minimal capability API, and requests route over pluggable anonymization transports ([mixnets](../primitives/mixnets.md) anticipated; KPS shipped). It covers the IP side of W-1 only: the endpoint still reads the query content ([pir](../primitives/pir.md) is the complement), bootstrapping needs a pre-existing RPC connection, and only a browser harness exists so far.

## Integration guide

Prerequisite: a browser context and a specifier contract address.

1. [app] Install the reference browser harness and point it at the specifier.
2. [harness] Reads the pinned hash on chain, fetches and verifies the bundle, sandboxes the worker.
3. [app] Issues ordinary JSON-RPC calls through the returned anonymized `fetch`.
4. [app] Trusts the on-chain hash only; resolvers and CDNs are unverified conveniences.

The [wallet integration guide](https://ethereum.github.io/anon-rpc/) and [SPEC.md](https://github.com/ethereum/anon-rpc) are the guide.

## Audit and maturity status

Draft specification (v0.3.0, July 2026) and reference prototype; no audits and no production deployments, with an audit on the workstream's Q3 2026 roadmap. MIT, TypeScript.

## Maintainer

The EF privacy team, at [ethereum/anon-rpc](https://github.com/ethereum/anon-rpc) (migrated from privacy-ethereum in 2026); background in the [announcement post](https://reads.ethereum.foundation/feed/anon-rpc/).
