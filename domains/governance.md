---
name: governance
last_reviewed: 2026-08-17
---

# Governance

Governance applications run collective decisions: proposals, voting, delegation, treasuries. Public ballots invite coercion, vote buying, and retaliation, and proposal access or vote inclusion must not depend on any single operator's goodwill.

## Functions

- Proposal submission and access
- Voting
- Delegation
- Tallying and execution
- Treasury management

## Guarantees

### G-1: Coercion-resistant ballots [P]

- **Property**: How a member voted is not visible to other voters, observers, or would-be buyers, during or after the vote, and the voter cannot prove their choice to a third party even when they want to, given an honest tallying coordinator and an uncoerced initial key registration. The tally stays public and provable; that a member voted remains visible.
- **Functions**: [Voting]
- **Motivation**: A public ballot prices retaliation and rewards purchase: an employer, a whale, or a buyer can check compliance vote by vote, and the checking is what makes the pressure work. Receipt-freeness removes the market for votes by making delivery unverifiable.
- **Primitives**: `private-voting` for receipt-free encrypted ballots with provable tallies.
- **Upgrade path**: Receipt-free encrypted voting with public provable tallies ran in production for public-goods funding from 2020 to 2023 and is entering DAO stacks as governance plugins. The residual trust is the coordinator, who sees how each member voted and can stall the count, but cannot censor messages or forge the tally. Threshold-FHE designs, at the pilot stage, replace that coordinator with a committee of ciphernodes: no single party can decrypt an individual ballot, and validity of encrypted votes is proven at submission.

### G-2: Permissionless proposals [CR]

- **Property**: Getting a proposal in front of voters requires meeting an open, objective bar (a token threshold, a deposit), never an operator's or moderator's approval.
- **Functions**: [Proposal submission and access]
- **Motivation**: Whoever curates the ballot governs before any vote happens. Agenda control is the quiet form of capture, and a moderated forum in front of an on-chain governor reintroduces the gatekeeper the governor removed.
- **Primitives**: None carded; threshold-based proposal rights are protocol design.
- **Upgrade path**: On-chain proposal thresholds are standard. The practical work is keeping the off-chain pipeline (forums, temperature checks) advisory rather than gating, and documenting the direct on-chain path.

### G-3: Censorship-resistant tallying [CR, S]

- **Property**: No operator can exclude a valid vote from the tally or stall the count, and anyone can verify the tally from public data.
- **Functions**: [Voting, Tallying and execution]
- **Motivation**: An off-chain tally under one operator is the election authority problem imported on chain: the operator can silently refuse votes at intake or go down at the decisive hour, and while the accepted record is publicly recountable, its completeness is not provable.
- **Primitives**: None carded for on-chain recording, which is protocol design; `private-voting` carries provable tallies where ballots are encrypted.
- **Upgrade path**: On-chain voting gives inclusion and recount by default. Off-chain schemes need published inputs and verifiable tallies to approach it, and the wallet-level broadcast path (W-4) is the floor under both.

### G-4: Votes bind execution [S]

- **Property**: A passed decision executes as voted, through timelocked on-chain execution. No discretionary layer between tally and effect can reinterpret it, veto it silently, or substitute its judgment.
- **Functions**: [Tallying and execution, Treasury management]
- **Motivation**: A vote that a multisig may or may not implement is a poll, and treasuries governed by polls are governed by the signers. Discretion between tally and execution is where governance quietly reverts to whoever holds the keys.
- **Primitives**: None carded; governor-plus-timelock execution is protocol design.
- **Upgrade path**: Timelocked execution from the governor contract is production-standard. Emergency powers, where they exist, deserve the treatment defi D-4 describes: public, delayed, bounded.

### G-5: Revocable, scoped delegation [S]

- **Property**: Delegating voting power is revocable by the member at any time, taking effect on every vote not yet snapshotted, can be scoped where the system allows, and never moves custody of the underlying assets.
- **Functions**: [Delegation]
- **Motivation**: Delegation that outlives the member's intent converts voice into property of the delegate. The member's exit from a bad delegate has to be cheaper than the delegate's use of their power.
- **Primitives**: None carded; delegation registries in the standard governor stack are protocol design.
- **Upgrade path**: Revocable token-holder delegation is the production default, and partial and time-scoped delegation ships in production on major governance stacks. Per-proposal-type scoping is where the design space still opens.

## Ship it

Integration guides by guarantee, added as tooling cards land:

- G-1, G-3: private-voting tooling (cards pending as DAO-stack plugins ship)
