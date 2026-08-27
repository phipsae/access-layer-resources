---
name: private-voting
last_reviewed: 2026-08-27
maturity: usable
guarantees: [G-1, G-3]

related:
  requires: []
  composes_with: [zk-group-membership]
  alternative_to: []
  see_also: [trusted-execution-environments]
---

# Private voting

## What it is

Encrypted ballots with a public, provable tally, and receipt-freeness on top: the voter cannot prove their own choice to a buyer even willingly, which is what removes the market for votes. Two trust models carry it. Coordinator-class systems encrypt votes to a tallying coordinator who decrypts, counts, and proves the count in zero knowledge. Committee-class systems tally homomorphically under threshold FHE, so no single party ever decrypts an individual ballot.

## What it guarantees

G-1: how a member voted stays hidden (from everyone but the coordinator in the first model, from everyone in the second), and receipts are unprovable by design. G-3: ballots enter on chain, so intake is censorship-resistant, and the tally comes with a proof anyone verifies.

## How it works

1. [user] Registers a voting key against their eligibility (a token snapshot, an anonymous group membership).
2. [user] Encrypts the ballot and submits it on chain; nobody can drop it at intake.
3. [user] The override window lets a voter change keys or votes, which is what defeats receipts: a buyer can never know the vote that counted.
4. [operator] The tally computes per model: the coordinator decrypts and proves, or the ciphernode committee evaluates homomorphically with ballot validity proven at submission.
5. [contract] The tally proof verifies on chain; the count is public, the ballots never are.

## Trust model

Coordinator-class: the coordinator sees individual votes and can stall the count, but cannot censor on-chain messages or forge the tally. Committee-class: privacy holds up to the collusion threshold of the committee, and liveness needs enough honest nodes; validity proofs on encrypted ballots keep garbage out without decryption.

## Known limits

- Participation stays visible; these systems hide choices, never turnout.
- Coercion at initial key registration precedes every guarantee; a buyer who controls the signup key wins before the first vote.
- The coordinator or committee is a liveness dependency at the decisive moment.
- Homomorphic tallying pays FHE costs; the committee model is at pilot stage.
- Adoption is thin: sustained use was public-goods funding (2020-2023); DAO-stack plugins are at demo stage.
- Post-quantum: the ZK layers are classical (forgeable under CRQC); the FHE flavor's lattice ciphertexts plausibly survive quantum harvest, unusually for recorded on-chain data.

## Implementations

[MACI](https://github.com/privacy-ethereum/maci) (PSE, coordinator model, powering clr.fund's funding rounds and an Aragon governance plugin at demo stage) and [Interfold](https://github.com/theinterfold/interfold) (formerly Enclave, threshold-FHE committee model, pilot). Tooling cards pending.

## Further reading

- [What is MACI?](https://maci.pse.dev/docs/introduction), PSE
- [MACI key change](https://maci.pse.dev/docs/core-concepts/key-change), the receipt-freeness mechanism
- [The Interfold documentation](https://docs.theinterfold.com/introduction), encrypted execution environments
- [Greco: zero-knowledge proofs of valid FHE RLWE ciphertexts](https://eprint.iacr.org/2024/594), Bottazzi, 2024
- [clr.fund](https://clr.fund/), the longest-running deployment
