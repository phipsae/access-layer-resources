---
name: maci
last_reviewed: 2026-09-09
maturity: usable
implements: [private-voting]
guarantees: [G-1, G-3]
---

# MACI

## What it implements

Coordinator-class [private-voting](../primitives/private-voting.md): encrypted ballots submitted on chain (G-3 intake), key-change overrides for receipt-freeness (G-1), and a coordinator who decrypts, tallies, and proves the count in zero knowledge. The coordinator sees individual votes and can stall the count; the deviation from the committee-class ideal is that single party.

## Integration guide

Prerequisite: a Node.js stack and a coordinator the vote's participants accept.

1. [app] Install the npm packages: `maci-contracts`, `maci-cli`, `maci-circuits`.
2. [app] Deploy the contracts and register eligibility (signup gatekeeper).
3. [user] Ballots encrypt and submit on chain; the override window stays open per poll.
4. [operator] The coordinator tallies and posts the proof; the contract verifies it.

The [documentation](https://maci.pse.dev/) is the guide.

## Audit and maturity status

Development ended in 2026: the repository and its satellite projects, including the Aragon governance plugin, are archived and read-only. Audited ([audit history](https://maci.pse.dev/docs/security/audit)); sustained production use was [clr.fund](https://clr.fund/)'s quadratic funding rounds, 2020 to 2023; the Aragon plugin reached Sepolia only. MIT; the packages remain installable and forkable.

## Maintainer

None active. Built by PSE (Ethereum Foundation), archived at [privacy-ethereum/maci](https://github.com/privacy-ethereum/maci) with docs at [maci.pse.dev](https://maci.pse.dev/).
