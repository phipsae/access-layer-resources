# Product Requirements Document

## Problem

Ethereum application builders have no single reference explaining which CROPS properties their applications can offer users, which primitives provide those properties, and how to integrate them. Knowledge is scattered across papers, specs, and tool documentation, each written for a different audience. The result: primitives that are production-ready go unadopted because the path from "why" to "how" is illegible.

## Product

An open knowledge base that takes an application builder from their domain (wallet, DEX, oracle) to the guarantees a CROPS-aligned application provides, the primitives behind them, and the tooling that ships them. Adoption is defined as integration: the product succeeds when a team ships a primitive in production, not when a page gets read.

## Users

1. **Application builders**: enter through their domain page, leave with an integration path. They own the information architecture.
2. **LLM-assisted workflows**: the repository is loaded as model context, by the maintaining team and by anyone else. This user owns the format: one concept per file, strict frontmatter, stable IDs, a glossary.

## Requirements

- **R-1 Content model**: four content types with a fixed flow. `domains/` (entry point: functions and guarantees) to `primitives/` (first-principles building blocks) to `tooling/` (implementations and integration guides). `specs/` holds reference cards.
- **R-2 Guarantees**: each domain decomposes into functions; desired properties are stated as guarantees with permanent IDs, declared as `### <ID>:` headings. Guarantees are descriptive, testable, and tagged with the CROPS letters they serve.
- **R-3 Maturity tracking**: primitives and tooling carry a maturity level (`research`, `concept`, `usable`, `production`), rated for the primitive itself wherever it runs, not only on Ethereum. Maturity flips are the mechanism by which described properties graduate to documented best practices.
- **R-4 Machine-readable structure**: frontmatter follows the per-type templates; cross-references use IDs and slugs, never restated text. A bare primitive slug in a guarantee block marks a planned card, tracked in this document's roadmap.
- **R-5 Descriptive voice**: properties and facts, no recommendations, no RFC-2119 keywords, no named applications in gap descriptions.

## Non-goals

- Not an accreditation body: no scoring, certifying, or endorsing of applications or vendors.
- No use-case catalog: end-user motivation lives inside each guarantee's why-it-matters line.
- No vendor advocacy: tooling cards are neutral and factual.

## Roadmap

### Milestone 1: format proof

- [x] Wallet domain end-to-end: intro, W-1..W-5 guarantee blocks fully drafted.
- [x] First primitive cards: `stealth-addresses` (W-2), `pir` (W-1, W-3), `mixnets` (W-1). One card fully drafted before replicating.
- [x] Document anon-rpc: first `tooling/` card. Private RPC access, satisfies W-1; linked from the wallet domain's Ship it section.

### Milestone 2: breadth

- [x] Domain stubs, one file per domain. Taxonomy validated 2026-08-17: a domain earns a page when its functions decompose differently and its guarantees are not a copy of another domain's. Bridges folded into `rollup`; naming folded into `identity`.
  - [x] `wallet` (functions and guarantees done)
  - [x] `defi`
  - [x] `payments`
  - [x] `identity`
  - [x] `oracle`
  - [x] `data-indexing`
  - [x] `rollup`
  - [x] `governance`
- [x] Fill functions and guarantees per domain:
  - [x] `defi`
  - [x] `payments`
  - [x] `identity`
  - [x] `oracle`
  - [x] `data-indexing`
  - [x] `rollup`
  - [x] `governance`
- [x] Contribution guidelines (`CONTRIBUTING.md`): how to add a card, guarantee-ID rules, voice and formatting rules, review flow.
- [x] Primitive backlog: complete 2026-08-19. 19 cards shipped: `light-clients`, `zkvm`, `shielded-pools`, `encrypted-mempools`, `anonymous-credentials`, `zk-group-membership`, `oprf`, `based-sequencing`, `decentralized-sequencing`, `validity-proofs`, `fraud-proofs`, `data-availability-sampling`, `private-rollups`, `tls-oracles`, `private-voting`, `native-account-abstraction`, `transaction-simulation`, plus the earlier `stealth-addresses`, `pir`, `mixnets`. Resolved without cards: `content-addressed-hosting` (prose in defi D-3), `omr` (mention in pir), `blob-bulletin-boards` (a concept, not yet a tool; SocialBlobs and ERC-8179/8180 references kept). One slug parked (see Parked).

### Milestone 3: publication (closed 2026-08-19; Vale deferred)

- [x] Markdown lint: markdownlint config and CI job.
- [ ] Prose lint: Vale styles for glossary terminology consistency and marketing-language bans. Deferred 2026-08-18: minimal stack chosen; revisit if outside contributions grow.
- [x] Link check: lychee CI action, internal and external links.
- Frontmatter and guarantee-ID validation: deliberately not built (2026-08-18). Structural checks (frontmatter fields, ID resolution, no duplicate IDs, `related`/`implements` slugs resolving) are a manual gate at draft time; the minimal lint stack keeps custom code out of the repo.
- [x] License: CC0 1.0.
- [x] GitHub org and go-public: pushed 2026-08-17 to `ethereum/app-enablement-resources`.

### Milestone 4: integration guides

Tooling cards, harvested from the domains' Ship it sections; the [ethereum.org developer-tools directory](https://ethereum.org/developers/tools/) is a standing candidate source. Cards land when the integration path is real per the tooling scope; the gates below keep announcements from counting as availability.

- [x] anon-rpc (W-1, X-1): done, the format proof.
- [x] kohaku: the wallet SDK surface (W-5 safety controls; D-2 and P-1 shielded-pool plugins; Railgun plugin operational, Privacy Pools in development).
- [x] Light-client integration: Helios and Colibri via the Kohaku provider layer (W-4, P-4, X-2); covered by the kohaku card.
- [x] Stealth payments: fluidkey-stealth-account-kit (W-2, P-1).
- [x] Anonymous membership: Semaphore (I-4, I-5).
- [x] Web proofs: TLSNotary (O-3, O-4). Reclaim excluded 2026-08-19: components of its documented integration path (the Solidity verifier among them) carry no license file, which fails the tooling scope's open-source requirement; AGPL parts are fine per the mandate. Revisit when licensed.
- [x] Encrypted-mempool access: Shutter opt-in RPC (D-1; Gnosis today, Ethereum PBS path).
- [ ] Private voting: the MACI Aragon plugin (G-1, G-3; gate: past demo stage).
- [ ] PIR endpoint tooling (W-1, X-1; gate: PIR Genesis, targeted Q4 2026).

### Parked

- Wallet guarantee candidates: key custody and recovery (open question: one merged key-access-lifecycle guarantee or two). Private transfers RESOLVED 2026-08-18: landed as payments P-1 with the required layered-on-transparent-L1 framing.
- Messaging domain, pending a sharper builder audience; revisit with XMTP, Status (Waku), and Push Protocol as the concrete apps.
- `verifiable-randomness` primitive card (O-3): inclusion uncertain per Yanis 2026-08-19, and the dominant oracle-style implementation needs a licensing check before it can be named (BUSL components are source-available, intolerable per the mandate); drand is the cleanly licensed beacon.
- BAL-based light-client spec: parked as a phase-2 authoring decision (2026-08-26). Gates: a proof-carrying successor to EIP-7928 post-values (the published diffs carry no proofs; correctness rests on executing validators rejecting false lists), stable JSON-RPC exposure of BALs, and observed client behavior after Glamsterdam activation. Design-space analysis lives in the workspace, outside this repository.

## Open questions

- ~~Domain taxonomy~~ (resolved 2026-08-17): eight app-vertical domains (wallet, defi, payments, identity, oracle, data-indexing, rollup, governance); messaging parked; bridges and naming folded into rollup and identity.
- ~~Tooling scope~~ (resolved 2026-08-17): open source software an application can integrate (libraries, SDKs, reference implementations, self-hostable services); end-user apps only via their integrable parts; paid hosted plans acceptable when the documented integration path is fully open source and self-hostable.
- Specs scope: phase 1 likely references external specs only (permalinks pinned to commit or tag); specifying solutions here would be a phase 2 decision.
- ~~Full primitive list beyond the initial three~~ (resolved 2026-08-19): 19 primitive cards shipped; additions now flow from guarantee needs rather than a list.
