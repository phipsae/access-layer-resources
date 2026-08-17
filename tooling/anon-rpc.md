---
name: anon-rpc
last_reviewed: 2026-08-17
maturity: usable
guarantees: [W-1]
---

# anon-rpc

## What it implements

anon-rpc is a standard, with a reference implementation, for making anonymized RPC requests from a browser context. It targets the gateway problem: browser-to-Ethereum traffic runs through a small set of HTTPS gateways and node-as-a-service providers, each able to log which addresses an IP asks about, and each able to refuse service.

The mechanism is a hash-pinned worker. A specifier contract publishes the hash of a worker bundle, with optional resolver entries (HTTPS URLs or KPS addresses); the harness fetches the bundle from any source, accepts any byte stream that matches the pinned hash, and runs it in a Web Worker inside a null-origin sandboxed iframe. The worker gets a minimal capability API (serving the host's fetch calls, outbound KPS connections, logging, scoped storage) and nothing else: no DOM, no wallet keys, no host identity. The spec and prototype ship one transport, KPS; [mixnets](../primitives/mixnets.md), Tor, and direct WebRTC links to node operators are the anonymization layers the design anticipates, each behind its own hash-pinned worker.

Toward W-1 this covers the IP side: the endpoint answering the query no longer learns who asked. It does not hide what was asked; the query content stays visible to the serving node, and [pir](../primitives/pir.md) is the complement being built in the same EF Private Reads workstream. Two further deviations from the ideal: bootstrapping needs a pre-existing RPC connection to read the specifier contract, and only a browser harness is implemented so far, though the spec provides for native ones (KPS over QUIC).

## Integration guide

1. [app] Installs the reference browser harness and points it at a specifier contract address.
2. [harness] Reads the pinned worker hash on chain, fetches the bundle from a resolver or mirror, verifies the hash, and starts the worker in its sandbox.
3. [app] Receives an anonymized `fetch` and issues ordinary JSON-RPC calls through it. The anonymization network follows from which specifier the app points at; the harness picks the carrier (WebRTC in browsers, QUIC natively).
4. [app] Treats the on-chain hash as the only trust anchor; resolver URLs and CDNs are unverified conveniences.

The [wallet integration guide](https://privacy-ethereum.github.io/anon-rpc/) covers specifier resolution, storage scoping, and worker updates.

## Audit and maturity status

Draft specification (v0.3.0, July 2026) and reference prototype, first published June 2026. No security audits and no production deployments; an audit sits on the workstream's Q3 2026 roadmap. Part of the EF Private Reads workstream, positioned by its authors within the Abstract Access Layer work.

## Maintainer

The EF privacy team, under the [privacy-ethereum](https://github.com/privacy-ethereum) organization. Code and spec at [privacy-ethereum/anon-rpc](https://github.com/privacy-ethereum/anon-rpc) (MIT, TypeScript); background in the [announcement post](https://reads.ethereum.foundation/feed/anon-rpc/).
