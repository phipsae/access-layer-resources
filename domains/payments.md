---
name: payments
last_reviewed: 2026-08-17
---

# Payments

Payment applications move value between people and businesses: transfers, invoicing, recurring billing, merchant settlement. On a transparent ledger, a single payment can expose a counterparty's balance, history, and future cash flow to the other side and to everyone else. Privacy here is a property layered onto a transparent-by-default L1, never a default to assume.

## Functions

- Sending and receiving payments
- Requesting and invoicing
- Recurring payments and subscriptions
- Settlement confirmation

## Guarantees

### P-1: Counterparty privacy on a transparent ledger [P]

- **Property**: Paying someone reveals neither the payer's balance and history to the payee, nor the payment relationship to anyone else. The privacy is a layer the application provides on a transparent-by-default L1, never an assumed default.
- **Functions**: [Sending and receiving payments]
- **Motivation**: A salary paid to a public address publishes the recipient's net worth to their employer, and every merchant sees the full history behind the address that pays them. Cash never did this; a payment app that does is leaking by design.
- **Primitives**: [stealth-addresses](../primitives/stealth-addresses.md) for receiving without a reusable identity; [shielded-pools](../primitives/shielded-pools.md) for amounts and balances.
- **Upgrade path**: Stealth-address payments run in production with minimal wallet-native support. Shielded pools cover amount privacy, with the anonymity set as the binding limit. The funding-link and scanning costs from the stealth-addresses card apply to payments in full.

### P-2: No intermediary can block a payment [CR]

- **Property**: Every payment the app can make through a processor, relayer, or API stays makeable as a direct on-chain transfer. The intermediated path is a convenience, never the only path.
- **Functions**: [Sending and receiving payments, Requesting and invoicing]
- **Motivation**: Processors are where payments get refused: account freezes, category bans, and jurisdiction blocks all operate at the processor layer. The direct path is the zero option that keeps the processor optional rather than structural.
- **Primitives**: None carded; direct transfer paths and self-hosted payment flows are design practices. Broadcast-level censorship belongs to wallet W-4; this guarantee is about the app adding no chokepoint of its own.
- **Upgrade path**: Keep a raw-transfer fallback behind every intermediated flow. Document how receiving addresses derive, so payees can verify them independently. Relayer- and paymaster-based flows need a user-broadcastable equivalent.

### P-3: Bounded recurring authority [S]

- **Property**: Any standing authority to pull funds is scoped to an amount, a period, and an expiry, and the user can revoke it at any time without the payee's cooperation.
- **Functions**: [Recurring payments and subscriptions]
- **Motivation**: Approval-based pull payments are the default subscription mechanism today, and the approvals are routinely unlimited: a blank check on that token, where one compromised or malicious payee contract drains its full balance. A subscription built on a blank check turns a billing relationship into a custody grant.
- **Primitives**: `native-account-abstraction`; session keys and scoped permissions through account abstraction.
- **Upgrade path**: EIP-7702, live since 2025, lets an ordinary account delegate to smart-account code that enforces scoped session keys: per-contract, capped, expiring. Wallet permission-request standards are draft but shipping in several wallets. Until integrated, per-payee capped approvals are the floor.

### P-4: Independently verifiable settlement [S, CR]

- **Property**: The payee can confirm a payment is final using public chain data alone, without trusting a payment API, an explorer, or the payer's word.
- **Functions**: [Settlement confirmation]
- **Motivation**: A merchant who learns "you were paid" from a hosted API has re-created the acquiring bank: the API can lie, go down, or be compelled, and the merchant ships goods against its word. Finality is on chain; reading it should not require permission.
- **Primitives**: [light-clients](../primitives/light-clients.md).
- **Upgrade path**: Light clients embedded in merchant tooling verify inclusion and finality locally. Finality lags around fifteen minutes, so point-of-sale flows either wait or accept inclusion with bounded risk; invoicing and settlement absorb the delay without noticing. Until integrated, cross-checking independent RPC endpoints is the weak-form fallback.

## Ship it

Integration guides by guarantee, added as tooling cards land:

- P-1: stealth-address payment tooling (card pending)
