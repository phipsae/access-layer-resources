---
name: anon-rpc
last_reviewed: 2026-08-17
maturity: usable
implements: [mixnets]
guarantees: [W-1]
---

# anon-rpc

## What it implements

anon-rpc is a standard, with a reference implementation, for making anonymized RPC requests from a browser context. It targets the gateway problem: browser-to-Ethereum traffic runs through a small set of HTTPS gateways and node-as-a-service providers, each able to log which addresses an IP asks about, and each able to refuse service.

The mechanism is a hash-pinned worker. A specifier contract publishes the hash of a worker bundle, with optional resolver URLs; the harness fetches the bundle from any source, accepts any byte stream that matches the pinned hash, and runs it in a Web Worker inside a null-origin sandboxed iframe. The worker gets a minimal capability API (fetch, KPS connections, logging, scoped storage) and nothing else: no DOM, no wallet keys, no host identity. Through it, requests route over pluggable anonymization transports, [mixnets](../primitives/mixnets.md) among them, alongside Tor and direct WebRTC connections to node operators.

Toward W-1 this covers the IP side: the endpoint answering the query no longer learns who asked. It does not hide what was asked; the query content stays visible to the serving node, and [pir](../primitives/pir.md) is the complement being built in the same EF Private Reads workstream. Two further deviations from the ideal: bootstrapping needs a pre-existing RPC connection to read the specifier contract, and the design is browser-first, with no native worker equivalent for Node, Bun, or Deno.

## Integration guide

1. [app] Installs the reference browser harness and points it at a specifier contract address.
2. [harness] Reads the pinned worker hash on chain, fetches the bundle from a resolver or mirror, verifies the hash, and starts the worker in its sandbox.
3. [app] Receives an anonymized `fetch` and issues ordinary JSON-RPC calls through it; transport selection happens inside the worker.
4. [app] Treats the on-chain hash as the only trust anchor; resolver URLs and CDNs are unverified conveniences.

The [wallet integration guide](https://privacy-ethereum.github.io/anon-rpc/) covers specifier resolution, storage scoping, and worker updates.

## Audit and maturity status

Draft specification and reference prototype, published June 2026. No security audits and no production deployments; the specifier resolver protocol is still being defined. Part of the EF Private Reads workstream, described by its authors as a prelude to the Abstract Access Layer architecture.

## Maintainer

The EF privacy team, under the [privacy-ethereum](https://github.com/privacy-ethereum) organization. Code and spec at [privacy-ethereum/anon-rpc](https://github.com/privacy-ethereum/anon-rpc) (MIT, TypeScript); background in the [announcement post](https://reads.ethereum.foundation/feed/anon-rpc/).
