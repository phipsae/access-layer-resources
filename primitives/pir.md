---
name: pir
last_reviewed: 2026-10-02
maturity: usable
guarantees: [W-1, W-3]

related:
  requires: []
  composes_with: [mixnets, stealth-addresses, light-clients]
  alternative_to: []
  see_also: []
---

# Private information retrieval (PIR)

## What it is

Private information retrieval lets a client fetch a record from a server's database without the server learning which record was fetched. The server computes an answer over data it holds, against a query that reveals nothing about the index. Two families exist: single-server schemes, where privacy rests on a cryptographic hardness assumption (today usually lattice-based), and multi-server schemes, where privacy is unconditional as long as the servers do not collude.

Every remote Ethereum read (a balance, a nonce, a log filter) tells the endpoint which addresses and contracts the user cares about. PIR removes that channel: the endpoint serves chain data while learning nothing about which part of it was requested. A sibling primitive, oblivious message retrieval, applies the same goal to detection: a server finds the messages addressed to you without learning which ones.

## What it guarantees

Satisfies the content side of W-1 (address-to-IP unlinkability): the queried addresses never reach the endpoint, so there is nothing to tie to the IP. Satisfies W-3 (no usage-data exfiltration) for lookups that must leave the device, such as token lists or price data, which stop describing the user's holdings when fetched via PIR.

## How it works

The stateless single-server flow:

1. [operator] Encodes the database into a PIR-servable structure and publishes the public parameters.
2. [wallet] Downloads the parameters before querying.
3. [wallet] Encrypts the index of the wanted record into a query the server cannot decrypt.
4. [operator] Computes the response over the entire encoded database, learning nothing about the index.
5. [wallet] Decrypts the response locally and recovers the record.

## Trust model

Single-server query privacy needs no honesty from the operator; it holds under the scheme's hardness assumption. The operator must only answer correctly, and answers over chain data can in principle be checked against light-client verified roots; retrieving the proofs privately is active design work. Multi-server schemes shift the assumption: privacy holds only while the servers do not collude. Every variant still shows the server that a query happened, when, and from which IP; `mixnets` cover that remainder.

## Known limits

- Server cost scales with the database: a stateless query touches the whole encoded shard, and hint-based schemes buy sublinear online work with a one-time client download and storage. Serving Ethereum-scale state takes sharding and heavy hardware; GPU benchmarks answer in tens of milliseconds over multi-gigabyte shards, at a few hundred kilobytes per query.
- Mutable state is the hard case: the encoding must be refreshed as the chain changes, which is expensive for hot state. Block-level access lists ([EIP-7928](https://eips.ethereum.org/EIPS/eip-7928), Glamsterdam) commit each header to a per-block diff of changed state, which supplies the refresh input; the refresh cost itself remains.
- A PIR query costs the operator far more than a plain RPC read, and who pays for that is unresolved.
- No sustained production deployment serves Ethereum reads yet; the EF Private Reads program targets a first end-to-end deployment over live state in Q4 2026.
- Post-quantum exposure is low. Lattice-based schemes rest on assumptions believed to withstand quantum attack, and multi-server schemes are information-theoretic, so even recorded queries stay private.

## Implementations

Open source libraries: [FrodoPIR](https://github.com/brave-experiments/frodo-pir), [SimplePIR](https://github.com/ahenzinger/simplepir). The EF [Private Reads](https://reads.ethereum.foundation/) program is building sharded PIR over live Ethereum state. Tooling cards pending.

## Further reading

- [EF Private Reads roadmap](https://reads.ethereum.foundation/roadmap/)
- [Sharded PIR design for the Ethereum state](https://ethresear.ch/t/sharded-pir-design-for-the-ethereum-state/24552), aliatiia, 2026
- [Ethereum privacy: private information retrieval](https://pse.dev/blog/ethereum-privacy-pir), PSE
- [InsPIRe: communication-efficient PIR with server-side preprocessing](https://eprint.iacr.org/2025/1352), Akhavan Mahdavi, Patel, Seo, Yeo, 2025
- [Oblivious message retrieval](https://eprint.iacr.org/2021/1256), Liu and Tromer, 2021
