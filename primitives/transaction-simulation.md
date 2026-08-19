---
name: transaction-simulation
last_reviewed: 2026-08-19
maturity: production
guarantees: [W-5]

related:
  requires: []
  composes_with: [light-clients]
  alternative_to: []
  see_also: []
---

# Transaction simulation

## What it is

Execute the candidate transaction against current chain state before signing it, and show the user what actually happens: which assets move, which approvals get granted, which state gets touched. It is the informed-consent primitive: the difference between signing bytes and signing an outcome.

## What it guarantees

W-5's inspectable half: the user sees the concrete effect of a signature before making it, from a source they control, with rules they can check. Simulation run locally keeps the inspection from becoming its own data leak.

## How it works

1. [wallet] Builds the candidate transaction.
2. [wallet] Simulates it against latest state: a local EVM, a self-hosted node, or the standard RPC method against a chosen endpoint.
3. [wallet] Diffs balances, approvals, and touched state before and after.
4. [user] Reads the outcome and signs or rejects.

## Trust model

A simulation is a prediction, never a promise. State can change between simulation and inclusion, and an adversarial contract can detect simulation context and behave differently under it than in the real block. A hosted simulator sees the user's intent before anything is broadcast, which is exactly the leak wallet W-3 describes; where the lookup leaves the device, the endpoint choice carries the trust.

## Known limits

- Time-of-check to time-of-use: the state the simulation ran against drifts before inclusion, and the drift can be adversarial.
- Simulation-aware contracts are a live pattern; a clean simulation is evidence, never proof.
- Hosted simulation APIs leak intent pre-broadcast.
- Gas estimates drift with state.
- The enforcement complement is in the EIP process: post-execution assertions (EIP-7906's state-diff opcodes, draft, built on EIP-8141's post-transaction frames) would revert a transaction on chain when its outcome violates stated constraints, enforcing what simulation could only predict; the two compose, contingent on frame transactions landing.

## Implementations

[eth_simulateV1](https://github.com/ethereum/execution-apis) (the standard execution-API method, shipped in clients) and [anvil](https://github.com/foundry-rs/foundry) (Foundry's local EVM). Tooling cards pending.

## Further reading

- [eth_simulateV1](https://ethereum.github.io/execution-apis/api/methods/eth_simulateV1/), execution-apis reference
- [EIP-7906: transaction assertions via state diff opcode](https://eips.ethereum.org/EIPS/eip-7906), the on-chain enforcement complement
- [Foundry book](https://getfoundry.sh/), anvil and local simulation tooling
