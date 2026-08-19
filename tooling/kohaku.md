---
name: kohaku
last_reviewed: 2026-08-19
maturity: usable
implements: [light-clients, shielded-pools]
guarantees: [W-4, W-5, D-2, P-1]
---

# Kohaku

## What it implements

The EF privacy wallet SDK: a provider abstraction that swaps between ethers, viem, and the [light-clients](../primitives/light-clients.md) Helios and Colibri, plus [shielded-pools](../primitives/shielded-pools.md) plugins: Railgun the most mature, Privacy Pools and Tornado Cash published as early alphas. Its own README says unaudited and not production-ready.

## Integration guide

Prerequisite: a TypeScript wallet or app.

1. [app] Install `@kohaku-eth/provider` and choose the backing: a light client for verified reads, or a plain provider.
2. [app] Add the shielded-pool plugin the flow needs (`@kohaku-eth/railgun`, `@kohaku-eth/privacy-pools`, `@kohaku-eth/tornado-cash`).
3. [app] Route reads and shielded operations through the Kohaku surface: one surface regardless of backing.

The [documentation](https://ethereum.github.io/kohaku/) is the guide.

## Audit and maturity status

SDK released May 2026, alpha packages, no audits yet; the Railgun plugin relays through ERC-4337. MIT per package manifests, with no repository-level license file yet.

## Maintainer

The Ethereum Foundation, at [ethereum/kohaku](https://github.com/ethereum/kohaku).
