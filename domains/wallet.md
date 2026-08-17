---
name: wallet
last_reviewed: 2026-08-17
---

# Wallet

A wallet is the user's interface to Ethereum. It holds keys, builds and signs transactions, queries the chain, and mediates every interaction with applications. That position concentrates risk: the wallet learns everything the user does before the chain does, and its defaults decide how much of that knowledge leaks and to whom. The guarantees below describe what a wallet can offer its users in CROPS terms.

## Functions

- Key management
- Network access (RPC / data queries)
- Transaction broadcast
- Address management
- Discovery and safety features

## Guarantees

### W-1: Address-to-IP unlinkability [P]

- **Property**: Network access does not reveal, to any single operator or observer, which addresses a user queries or controls, and does not tie those addresses to the user's IP address.
- **Functions**: [Network access]
- **Motivation**: A hosted RPC endpoint sees every balance check and every pending transaction, together with the IP they came from. Whoever runs it can build a financial profile of the user, sell it, or be forced to log it for someone else. <!-- TODO: replace with sourced/real user motivation -->
- **Primitives**: `pir` for chain queries, `mixnets` for network-level cover.
- **Upgrade path**: Light clients (Helios, Colibri, both packaged in the Kohaku provider layer) verify chain data locally, which removes trust in the endpoint's answers; the endpoint still sees which addresses are queried. PIR for chain queries, which would close that leak, remains research.

### W-2: Address-to-address unlinkability [P]

- **Property**: Operating the wallet does not link a user's addresses to each other, on chain or through the wallet's own queries.
- **Functions**: [Address management]
- **Motivation**: One address receives a salary, another makes donations. If the two can be joined, a single payment to one exposes everything behind the other. Deriving accounts from one seed and querying them over the same connection joins them at the network layer even when they stay separate on chain. <!-- TODO: replace with sourced/real user motivation -->
- **Primitives**: `stealth-addresses` (ERC-5564) for receiving without publishing a shared identity; per-context accounts with isolated querying (wallet practice, no card).
- **Upgrade path**: Stealth addresses are specified in ERC-5564 (Final), with production deployments (Umbra, Fluidkey) on mainnet and several L2s; wallet-native adoption remains minimal.

### W-3: No usage-data exfiltration [P, S]

- **Property**: Usage data (addresses, balances, history, dapp activity, feature usage) leaves the device only with the user's explicit consent, given per destination and purpose.
- **Functions**: [Discovery and safety features]
- **Motivation**: Telemetry, price feeds, token-list fetches, and push notifications each look harmless in isolation; together they mirror the portfolio to third parties the user never chose. Burying the consent step in a settings page means nobody actually reads it. <!-- TODO: replace with sourced/real user motivation -->
- **Primitives**: `pir` where a lookup has to leave the device; local-first design covers the rest and needs no card.
- **Upgrade path**: Achievable today by design: keep processing local and gate each outbound lookup on explicit consent.

### W-4: Zero option on every intermediated path [CR]

- **Property**: Every wallet function that routes through an intermediary (hosted RPC, relayer, bundler, paymaster) keeps an intermediary-free path that stays credible and accessible.
- **Functions**: [Network access, Transaction broadcast]
- **Motivation**: A convenience path turns into a chokepoint the day its operator starts filtering, and users locked to it at that point have no exit. The intermediary-free path has to exist and stay usable earlier, while nobody needs it yet. <!-- TODO: replace with sourced/real user motivation -->
- **Primitives**: `light-clients`; `native-account-abstraction` (proposal stage) would remove relayer dependence; self-hosted nodes and direct p2p broadcast are infrastructure practices, no card.
- **Upgrade path**: Self-hosted nodes and light clients give network access its zero option, and direct broadcast paths exist. Permissionless alternatives to bundlers (and to paymasters, where gas is sponsored) are still early.

### W-5: User-controlled safety features [S, O]

- **Property**: Filters, warnings, simulations, and any AI assistance run under the user's control: rules are inspectable, decisions can be overridden, and nothing reports home by default.
- **Functions**: [Discovery and safety features]
- **Motivation**: Safety tooling the user can neither inspect nor override decides on their behalf what they may sign. A wallet that silently blocks a contract has made a custody decision, whatever the intent behind it. <!-- TODO: replace with sourced/real user motivation -->
- **Primitives**: `transaction-simulation` run locally; locally verifiable filters and community-maintained lists with override paths (no card yet).
- **Upgrade path**: Local transaction simulation is available today; risk-based transaction controls under user control are part of Kohaku's stated scope.

## Ship it

Integration guides by guarantee, added as tooling cards land:

- W-1: anon-rpc (tooling card pending)
- W-5: Kohaku SDK (tooling card pending)
