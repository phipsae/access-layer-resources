---
name: wallet
last_reviewed: 2026-08-14
---

# Wallet

TODO: intro paragraph.

## Functions

- Key management
- Network access (RPC / data queries)
- Transaction broadcast
- Address management
- Discovery and safety features

## Guarantees

### W-1: Address-to-IP unlinkability [P]

TODO. Network access does not reveal which addresses a user queries or controls to any network observer or RPC provider.

### W-2: Address-to-address unlinkability [P]

TODO. Wallet operation does not link a user's addresses to each other.

### W-3: No usage-data exfiltration [P, S]

TODO. Usage data does not leave the device without explicit consent.

### W-4: Zero option on every intermediated path [CR]

TODO. Every function that routes through an intermediary (RPC provider, relayer, paymaster) keeps a credible, accessible intermediary-free path.

### W-5: User-controlled safety features [S, O]

TODO. Filters, warnings, and any AI assistance are locally verifiable, transparent in their rules, overridable, and private by default.

## Ship it

TODO: links to tooling/ once cards exist.
