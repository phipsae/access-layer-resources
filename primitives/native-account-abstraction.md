---
name: native-account-abstraction
last_reviewed: 2026-10-02
maturity: concept
guarantees: [W-4, P-3]

related:
  requires: []
  composes_with: []
  alternative_to: []
  see_also: []
---

# Native account abstraction

## What it is

Account abstraction enforced by the protocol itself: smart accounts become first-class transaction senders, with validation and execution defined by the protocol rather than emulated by outside infrastructure. The current L1 direction is frame transactions (EIP-8141): a transaction type decomposed into contract-executable frames, so arbitrary account code validates, pays gas, and batches, generalizing the earlier dedicated-transaction-type designs (EIP-7701 on L1, RIP-7560 for rollups).

## What it guarantees

W-4: the bundler and relayer layer that extra-protocol account abstraction requires leaves the path entirely; a smart-account transaction propagates through the ordinary public mempool and any proposer includes it. P-3's substrate: scoped session keys, spending caps, and expiries live in account code the protocol honors directly.

## How it works

1. [user] The account carries its validation code; ordinary accounts get default code rather than deploying anything.
2. [user] Sends the frame transaction: validation frames, payment frames, execution frames.
3. [node] The public mempool propagates it under protocol validation rules built to stay DoS-safe.
4. [node] Any proposer includes it; no special actor exists to refuse.
5. [contract] The protocol runs validation frames, then execution, atomically.

## Trust model

Consensus itself; nothing else is introduced. The engineering surface, and the reason this is hard to ship, is keeping arbitrary validation code DoS-safe in a public mempool.

## Known limits

- EIP-8141, the EIP bringing native account abstraction, is a Hegotá headliner per [EIP-8081](https://eips.ethereum.org/EIPS/eip-8081); keyed nonces ([EIP-8250](https://eips.ethereum.org/EIPS/eip-8250)) and recent roots ([EIP-8272](https://eips.ethereum.org/EIPS/eip-8272)) are considered for inclusion. No activation date is set.
- Until then, account abstraction runs extra-protocol through bundler markets, an intermediated present; EIP-7702 gives EOAs account code today without removing the relay dependence of sponsored flows.
- Post-quantum: frames are the modular piece added to accounts, so post-quantum validation can be adopted later per account, without a protocol fork.

## Implementations

EIP-8141 together with EIP-8250, EIP-8272 and FOCIL (EIP-7805) runs on the [ethrex Hegotá devnet](https://github.com/lambdaclass/ethrex/tree/6f9f055a41883c4d996ff17b8a37174ff7524527).

## Further reading

- [EIP-8141: frame transaction](https://eips.ethereum.org/EIPS/eip-8141), the current direction
- [EIP-8081: Hegotá network upgrade meta](https://eips.ethereum.org/EIPS/eip-8081)
- [EIP-8250: keyed nonces for frame transactions](https://eips.ethereum.org/EIPS/eip-8250)
- [EIP-8272: recent roots for frame transactions](https://eips.ethereum.org/EIPS/eip-8272)
- [EIP-7701: native account abstraction](https://eips.ethereum.org/EIPS/eip-7701) (withdrawn, superseded by 8141)
- [RIP-7560: native account abstraction for rollups](https://docs.erc4337.io/core-standards/rip-7560.html)
- [ERC-4337: account abstraction using alt mempool](https://eips.ethereum.org/EIPS/eip-4337)
- [EIP-7702: set EOA account code](https://eips.ethereum.org/EIPS/eip-7702)
