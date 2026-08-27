---
name: social-recovery
last_reviewed: 2026-08-27
maturity: production
guarantees: [W-6]

related:
  requires: []
  composes_with: []
  alternative_to: []
  see_also: [native-account-abstraction]
---

# Social recovery

## What it is

Social recovery separates spending from recovering. A smart account designates a set of guardians (other devices, hardware keys, trusted people, or services), and an M-of-N quorum of them can authorize one action only: rotating the account's signing key. Guardians never spend. A time delay sits between a recovery request and its execution, so the legitimate owner can cancel a rotation they did not start.

The construction requires an account whose validation logic is programmable, which is smart-account territory ([native-account-abstraction](native-account-abstraction.md) tracks the protocol side); a bare EOA cannot express it.

## What it guarantees

W-6: a lost device or key stops being a lost account, because the guardian quorum restores access by rotation; no single custodian can move funds or block recovery, because guardians hold a narrow rotation power behind a threshold and a veto window. The linkage half of W-6 depends on the guardian design: publicly listed guardians tie accounts together and to identities, and guardian-hiding variants keep the set private until a recovery actually runs.

## How it works

1. [user] Configures guardians and a threshold on the smart account, with a recovery delay.
2. [user] Loses the signing device.
3. [wallet] From a fresh device, requests recovery to a new signing key.
4. [relayer] Guardians approve until the threshold is met; the request enters its delay window, visible to the account owner.
5. [contract] After the delay, executes the rotation; the account continues under the new key, with history and address intact.

## Trust model

An honest-majority assumption over the guardian set, twice: fewer than M guardians collude (otherwise they can rotate the account to a key they control, subject to the veto window), and at least M stay reachable (otherwise recovery is unavailable and the account degrades to its remaining keys). The delay window is the countermeasure to guardian compromise and the cost of every legitimate recovery. Guardians are chosen by the user; the quality of the assumption is the quality of that choice.

## Known limits

- Guardian collusion at threshold is account takeover, rate-limited only by the veto delay; the delay helps only an owner who still holds some key and watches the chain.
- Guardian loss compounds silently: each unreachable guardian lowers the effective threshold margin, and nothing forces re-configuration.
- The recovery window is a social-engineering surface: an attacker who can rush the user into approving a rotation inherits the account.
- On-chain guardian sets are public: they link the account to its guardians' identities and to any other account naming the same guardians, which is the linkage exposure W-6 names. Guardian-hiding designs (hashed guardian commitments, ZK proofs of guardianship at recovery time) close it at added circuit and UX cost.
- Post-quantum exposure follows the signature schemes of the account and its guardians, classical today; rotation itself is the migration mechanism, since a quorum can rotate accounts to post-quantum keys without address changes.

## Implementations

[Argent guardian contracts](https://github.com/argentlabs/argent-contracts) (guardian recovery on mainnet since 2020, primary deployment now on an L2), [Candide's social recovery module](https://github.com/candidelabs/candide-contracts) (module for Safe-class smart accounts), [ZK Email recovery](https://github.com/zkemail/email-recovery) (email-based guardians proven in zero knowledge). Tooling cards pending.

## Further reading

- [Why we need wide adoption of social recovery wallets](https://vitalik.eth.limo/general/2021/01/11/recovery.html), Buterin, 2021
- [ERC-7093: social recovery interface](https://eips.ethereum.org/EIPS/eip-7093)
- [ZK Email documentation](https://docs.zk.email/)
