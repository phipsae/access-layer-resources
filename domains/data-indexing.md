---
name: data-indexing
last_reviewed: 2026-08-17
---

# Data indexing

Indexing applications serve the chain's read side: explorers, indexers, query APIs. Whoever serves reads learns which addresses and contracts a user watches, and sits in a position to shape or omit what gets shown as the chain's contents.

## Functions

- Serving chain reads
- Indexing and derived data
- Search and discovery
- Archival and reconstruction

## Guarantees

### X-1: Private reads [P]

- **Property**: Serving a query teaches the service nothing about which addresses, contracts, or topics the user watches, and nothing ties the query stream to the user's IP.
- **Functions**: [Serving chain reads, Search and discovery]
- **Motivation**: The read side sees the questions, and the questions are the profile: which addresses a user checks daily says more than what the chain records about them. A query log is surveillance the user cannot see happening.
- **Primitives**: [pir](../primitives/pir.md); [mixnets](../primitives/mixnets.md) for the transport half.
- **Upgrade path**: Transport-level cover (mixnets, anonymized RPC) hides who is asking today. PIR closes what is asked, with the first end-to-end deployment over live state targeted for Q4 2026. Serving PIR is a cost decision the operator makes, which is the adoption problem this repository exists to work on.

### X-2: Verifiable answers [S]

- **Property**: A consumer can verify every answer: raw state against light-client verified roots, derived data against proofs that the stated transform ran over the full canonical input range, with the inputs bound to light-client verified headers. Execution proofs alone do not rule out omitted inputs; completeness is part of the claim being proven.
- **Functions**: [Serving chain reads, Indexing and derived data]
- **Motivation**: An indexer's answer is taken on faith by default, and the faith is misplaceable in both directions: a wrong balance ships goods, a wrong history convicts an address. The chain is verifiable; reads of it should not launder that away.
- **Primitives**: `light-clients`; `zkvm` for proven derived queries.
- **Upgrade path**: Inclusion and state proofs on responses are the floor. Light clients verify them locally. zkVM-proven derived data is emerging, on the same trajectory the oracle page tracks for aggregation.

### X-3: Reproducible datasets [CR, O]

- **Property**: Anyone can rebuild the served dataset from public data: transform code open source, schemas documented, builds deterministic. Where source data expires (blobs prune after roughly 18 days, and a default node holds only a sample of them since data-availability sampling), the retention need is stated so anyone running full-custody archival can meet it.
- **Functions**: [Indexing and derived data, Archival and reconstruction]
- **Motivation**: An index nobody can rebuild is a source of truth with an owner. When the operator changes terms, errs, or disappears, every downstream app inherits the outage and nobody can check the successor.
- **Primitives**: None carded; open pipelines and deterministic builds are engineering practice.
- **Upgrade path**: Publish the pipeline with the API. Pin transform versions. Treat blob retention as part of the public dataset, since pruned data is only as public as its archives.

### X-4: Transparent filtering [CR, S]

- **Property**: The served view never silently omits or editorializes chain contents. Any filtering (spam, legal) is disclosed where it applies, and an unfiltered path to the same data stays reachable.
- **Functions**: [Serving chain reads, Search and discovery]
- **Motivation**: Explorers and search are the chain's de facto interface: an address that a major explorer hides has been censored in every way that matters socially, while the chain pretends otherwise. Silent curation of reads is censorship of perception.
- **Primitives**: None carded; disclosed filters with override paths are practice, the same stance wallet W-5 takes for safety tooling.
- **Upgrade path**: Label filtered results at the point of omission rather than in a policy page. Keep raw-data endpoints unfiltered. Make filter lists themselves public and diffable.

## Ship it

Integration guides by guarantee, added as tooling cards land:

- X-1: [anon-rpc](../tooling/anon-rpc.md) for the transport half; PIR tooling pending
