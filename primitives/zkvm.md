---
name: zkvm
last_reviewed: 2026-08-19
maturity: production
guarantees: [O-3, X-2]

related:
  requires: []
  composes_with: [light-clients, validity-proofs]
  alternative_to: []
  see_also: []
---

# zkVM

## What it is

A zkVM proves that a program executed correctly: ordinary code, compiled to a provable instruction set (usually RISC-V), runs once on a prover, and anyone can verify the resulting proof in milliseconds without re-running anything. It replaces hand-built zero-knowledge circuits with a general-purpose target that regular toolchains compile to.

## What it guarantees

Whoever verifies the proof knows the output is the stated program applied to the stated inputs, whoever ran it. For O-3 that makes an oracle's aggregation pipeline checkable from outside; for X-2 it makes derived chain data (an index, a history, a statistic) provable rather than taken on the indexer's word. Verification is cheap enough to run in a contract or a client.

## How it works

1. [developer] Compiles the program to the zkVM's instruction set and publishes the program hash.
2. [operator] Executes the program on the inputs and generates the proof, on hardware sized to the latency target.
3. [operator] Optionally aggregates many proofs into one through recursion.
4. [contract] Verifies the proof against the program hash and the public inputs, and accepts the output.

## Trust model

Two layers must be sound: the proof system's cryptography, and the zkVM's implementation of it, circuits and compiler included. A toolchain bug means false proofs verify, which is why the ecosystem's mitigation is plurality: multiple independently built zkVMs proving the same computations, and Ethereum's own proving track is designed around several proving systems rather than one. The prover can withhold a proof; it can never forge one the verifier accepts, up to those two layers holding.

## Known limits

- Proving costs orders of magnitude more than native execution; real-time proving of Ethereum blocks takes GPU clusters, and latency budgets shape what is provable in practice.
- Programs must be deterministic; anything touching randomness or external calls needs restructuring.
- The toolchains are young and the circuits are the attack surface; audits and cross-implementation checks carry more weight than the underlying math.
- Post-quantum exposure is low for hash-based STARK proofs, which rest on assumptions expected to survive a CRQC; pairing-based SNARK wrappers used to shrink proofs for on-chain verification are not, and can be swapped without changing the programs.

## Implementations

[SP1](https://github.com/succinctlabs/sp1) (demonstrated real-time proving of Ethereum blocks in 2025, production release February 2026), [RISC Zero](https://github.com/risc0/risc0), [Jolt](https://github.com/a16z/jolt) (research-stage). The EF zkEVM track coordinates proving of Ethereum blocks across independent zkVMs. Tooling cards pending.

## Further reading

- [SP1 Hypercube: proving Ethereum in real time](https://blog.succinct.xyz/sp1-hypercube/), Succinct
- [RISC Zero zkVM documentation](https://dev.risczero.com/api/zkvm/)
- [Jolt: SNARKs for virtual machines via lookups](https://eprint.iacr.org/2023/1217), Arun, Setty, Thaler, 2023
- [ethproofs.org](https://ethproofs.org/), live tracking of Ethereum block proving
- [EF zkEVM track](https://zkevm.ethereum.foundation/), the path to consensus-level proof verification
