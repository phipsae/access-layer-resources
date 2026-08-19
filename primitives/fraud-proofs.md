---
name: fraud-proofs
last_reviewed: 2026-08-19
maturity: production
guarantees: [R-3]

related:
  requires: []
  composes_with: []
  alternative_to: [validity-proofs]
  see_also: []
---

# Fraud proofs

## What it is

An optimistic rollup accepts posted state roots without proof, then holds them open to challenge for a window. Any watcher who spots a wrong root can dispute it, bisecting the disagreement down to a single instruction that L1 executes to settle who lied.

## What it guarantees

R-3 under a watchfulness assumption: no operator's word defines balances as long as one honest party checks the chain and can act. The system is exactly as trustless as its least censorable challenger.

## How it works

1. [operator] Posts a state root optimistically; the challenge window opens.
2. [node] Watchers re-execute the batch from published data and compare.
3. [operator] A challenger who disagrees posts a bond and opens a dispute.
4. [contract] Interactive bisection narrows the dispute to one step; L1 executes that step and slashes the loser.
5. [contract] Unchallenged roots finalize when the window closes, typically around seven days.

## Trust model

One honest, funded challenger who can get transactions included on L1 for the whole window. Everything reduces to that: the operator's honesty is unnecessary, but somebody's vigilance is not. Permissionless-validation designs close the delay-attack hole where rich adversaries could stall disputes indefinitely.

## Known limits

- The window is the price: withdrawals wait days unless a liquidity provider fronts them, for a fee.
- Challenge games must survive censorship and resource exhaustion during the window; the modern designs are built around exactly that.
- Watchtower economics are unsolved in general: verification is a public good, and unrewarded watchers are the assumption doing quiet work.
- Post-quantum: largely resilient by construction; disputes re-execute code rather than lean on quantum-fragile cryptography, so the exposure is the underlying chain's, not the mechanism's.

## Implementations

[BoLD](https://github.com/OffchainLabs/bold) (permissionless validation on Arbitrum since February 2025) and [Cannon](https://github.com/ethereum-optimism/optimism/tree/develop/cannon) (OP Stack fault proofs). Tooling cards pending.

## Further reading

- [BoLD: a gentle introduction](https://docs.arbitrum.io/how-arbitrum-works/bold/gentle-introduction), Arbitrum
- [Fault proofs explainer](https://docs.optimism.io/stack/fault-proofs/explainer), Optimism
- [An incomplete guide to rollups](https://vitalik.eth.limo/general/2021/01/05/rollup.html), Vitalik Buterin, 2021
