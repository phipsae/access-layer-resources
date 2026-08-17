---
name: mixnets
last_reviewed: 2026-08-17
maturity: production
guarantees: [W-1]

related:
  requires: []
  composes_with: [pir, stealth-addresses]
  alternative_to: []
  see_also: []
---

# Mixnets

## What it is

A mixnet routes traffic as fixed-size, layered-encrypted packets through independent relays that delay and reorder them before forwarding. Clients transmit at a constant rate, sending cover packets when they have nothing real to say. An observer watching every wire in the network sees uniform packets flowing at uniform rates and cannot match individual senders to destinations.

That observer is the difference from low-latency onion routing such as Tor, which protects against any single relay but yields to timing correlation by someone watching both ends. Mixnets spend latency and bandwidth to defeat exactly that adversary.

## What it guarantees

Satisfies the network side of W-1 (address-to-IP unlinkability): chain queries and transaction broadcasts reach the endpoint without carrying the user's IP, and a global observer cannot trace them back. The endpoint still reads the request content, queried addresses included; `pir` closes that half, which is why the two compose.

## How it works

1. [wallet] Splits the request into fixed-size packets and picks a route through the relay layers (source routing).
2. [wallet] Wraps each packet in one encryption layer per hop, so every packet on the network looks identical.
3. [wallet] Sends at a constant rate, slotting real packets into a stream of cover packets.
4. [node] Each mix strips its layer, holds the packet for a random delay, and forwards it reordered among others.
5. [operator] The exit gateway forwards the request to its destination as a proxy, exposing its own IP in place of the user's.
6. [wallet] The response returns through the mixnet along a reply route the wallet included in the packet.

## Trust model

One honest mix per route is enough: a single relay that genuinely delays and reorders breaks the traceable chain. The entry gateway sees the user's IP but not the destination; the exit gateway sees the destination but not the IP; deanonymization requires collusion across the full route. Availability depends on the gateways, and an exit gateway can refuse to forward to a given destination.

## Known limits

- Latency adds hundreds of milliseconds to seconds per round trip, depending on mixing parameters. Transaction broadcast tolerates that; interactive dapp reads mostly do not.
- Cover traffic costs constant bandwidth whether or not the user is active.
- The anonymity set is the set of concurrent users; a quiet mixnet protects little, and long-running traffic patterns remain exposed to statistical disclosure attacks that per-message unlinkability does not stop.
- Post-quantum exposure sits in the packet format: its key agreement is classical elliptic-curve, so transcripts captured at compromised nodes, or recorded on the wire before channel protections, can be retroactively unwrapped by a CRQC. Nym began deploying post-quantum key exchange on the links between hops in 2026; post-quantum packet formats themselves are research-stage.

## Implementations

[Nym](https://github.com/nymtech/nym) has operated a general-purpose mixnet in production since 2022 (entry gateway, three mix layers, exit gateway); wallets reach it today through a SOCKS5 proxy rather than native integration. For Ethereum RPC, [anon-rpc](../tooling/anon-rpc.md) exposes mixnets as one of its pluggable transports.

## Further reading

- [The Loopix anonymity system](https://arxiv.org/abs/1703.00536), Piotrowska et al., 2017; the design Nym builds on
- [Nym traffic flow documentation](https://nym.com/docs/network/mixnet-mode/traffic-flow)
- [Nym whitepaper](https://nym.com/nym-whitepaper.pdf)
