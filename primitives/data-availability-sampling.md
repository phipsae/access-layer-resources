---
name: data-availability-sampling
last_reviewed: 2026-08-19
maturity: production
guarantees: [R-4]

related:
  requires: []
  composes_with: [light-clients]
  alternative_to: []
  see_also: []
---

# Data availability sampling

## What it is

Data availability sampling lets a node verify that published data exists without downloading it. The data is erasure-coded so any half reconstructs the whole, split into columns of cells verifiable against polynomial commitments, and each node checks the pieces assigned to it. If the pieces are there across the population, with overwhelming probability all of it is.

## What it guarantees

R-4 at scale: the chain can carry more data than any single node's bandwidth while everyone still verifies that reconstruction stays possible. Withholding data means fooling the aggregate of samplers, and partial withholding fails against erasure coding.

## How it works

1. [operator] Publishes blob data erasure-coded into columns of cells, each cell verifiable against the blobs' KZG commitments carried by the block.
2. [node] Every node custodies a pseudo-random subset of columns derived from its node ID, and serves them.
3. [node] Each slot, nodes download the samples assigned to them; a missing piece is an alarm, and the randomness lies across the node population, so withholding must fool the aggregate.
4. [node] If pieces go missing, any half of the columns reconstructs the rest.

## Trust model

Enough independent custody and honest sampling across the network; the commitments bind what was promised to what is served, so a byte swapped is a proof failed. No single node vouches for availability; the aggregate does.

## Known limits

- Default nodes no longer hold whole blobs; archival beyond the retention window is a separate, full-custody role.
- Per-node assurance is probabilistic and only strong in aggregate; a partitioned or eclipse-attacked node can be fooled in isolation.
- Blob data prunes after roughly 18 days regardless; sampling proves availability, never permanence.
- Post-quantum: KZG commitments are pairing-based, so the commitment layer eventually needs a hash-based replacement, an active research direction; the erasure-coding and sampling logic carry over unchanged.

## Implementations

Live on Ethereum since the Fusaka upgrade (December 2025) as PeerDAS, implemented across the consensus clients; blob capacity has been raised repeatedly since. Spec: [fulu/das-core](https://github.com/ethereum/consensus-specs/blob/master/specs/fulu/das-core.md). Tooling cards pending.

## Further reading

- [EIP-7594: PeerDAS](https://eips.ethereum.org/EIPS/eip-7594)
- [Fusaka mainnet announcement](https://blog.ethereum.org/2025/11/06/fusaka-mainnet-announcement), Ethereum Foundation
- [Danksharding](https://ethereum.org/en/roadmap/danksharding/), ethereum.org
- [Data availability sampling: from basics to open problems](https://www.paradigm.xyz/2022/08/das), Paradigm, 2022
