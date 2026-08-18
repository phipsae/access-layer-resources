---
name: defi
last_reviewed: 2026-08-17
---

# DeFi

DeFi applications let users trade, lend, borrow, and provide liquidity through smart contracts. Positions and order flow are public by default, so intent can be extracted as MEV before execution, and most access runs through hosted frontends that can be censored or altered. The domain will later split into finer categories (dex, lending, derivatives) if their guarantees diverge.

## Functions

- Trading (order submission and execution)
- Lending and borrowing
- Investing and liquidity provision
- Market data consumption
- Frontend distribution

## Guarantees

### D-1: Pre-execution order privacy [P]

- **Property**: No party can read an order between the moment the user forms it and the moment its execution is fixed while being in a position to act on it first.
- **Functions**: [Trading]
- **Motivation**: Visible intent is extractable intent. An order that crosses a public mempool or a logging RPC can be sandwiched or front-run by whoever reads it, and the extraction needs no protocol bug, only the read.
- **Primitives**: `encrypted-mempools`; [mixnets](../primitives/mixnets.md) for submission-side network cover.
- **Upgrade path**: Private order routing is available today and shifts trust to the receiving builder rather than removing it. A threshold-encrypted mempool has run in production on Gnosis Chain since 2024, opt-in, and the approach is moving toward Ethereum's PBS pipeline. Batch auctions with a uniform clearing price remove the value of ordering within a batch.

### D-2: Position privacy as an explicit layer [P]

- **Property**: Holding or adjusting a position does not publish the user's balances, strategy, or liquidation levels. The privacy is a layer the application offers on a transparent-by-default L1, never an ambient default.
- **Functions**: [Lending and borrowing, Investing and liquidity provision]
- **Motivation**: A public liquidation price is a target and a public portfolio is a profile. Visible positions get hunted toward their liquidation levels, copied, and correlated with everything else their owner ever did on chain.
- **Primitives**: `shielded-pools`; [stealth-addresses](../primitives/stealth-addresses.md) for receiving proceeds without re-linking them.
- **Upgrade path**: Shielded pools have run in production on mainnet since 2021; association-set designs that gate illicit inflows ship since 2023. The binding limit is the anonymity set: private traffic remains a marginal share of Ethereum activity, and a small crowd hides poorly.

### D-3: Zero option on access and exit [CR]

- **Property**: Every action the hosted frontend offers stays possible without it: contract interfaces are documented, the frontend builds and mirrors from source, and no exit depends on an operator-run keeper or backend.
- **Functions**: [Frontend distribution, Lending and borrowing, Investing and liquidity provision]
- **Motivation**: The frontend is where DeFi gets censored in practice. Hosted interfaces have been geo-blocked and delisted while their contracts kept running, and users who only knew the frontend were locked out of their own positions.
- **Primitives**: `content-addressed-hosting`; direct contract interaction and community mirrors are practices, no card.
- **Upgrade path**: Verified source and documented ABIs set the floor. Content-addressed frontends make mirrors cheap and tamper-evident. The exit path earns the same testing as the happy path, while nobody needs it yet.

### D-4: No hidden admin switches [S, O]

- **Property**: Every power that can upgrade, pause, reprice, or move user funds is publicly listed, delayed long enough for users to exit before it takes effect, and impossible to exercise silently.
- **Functions**: [Lending and borrowing, Investing and liquidity provision, Market data consumption]
- **Motivation**: Most major DeFi protocols retain upgrade, pause, or repricing powers behind someone's keys, and an instant silent switch is how contract-level rug pulls work. Users deposited against the code they read; a silent upgrade means the code they read is not the code holding their money.
- **Primitives**: None carded; timelocks, immutable cores, and on-chain governance are design practices.
- **Upgrade path**: Immutable core contracts where the design allows; timelocks on every power that touches funds, oracle selection included; verified source so the switches are visible at all.

## Ship it

Integration guides by guarantee, added as tooling cards land:

- D-1: encrypted-mempool tooling (card pending)
- D-2: shielded-pool tooling (cards pending; the Kohaku plugins are candidates)
