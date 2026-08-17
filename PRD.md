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

- [ ] Domain stubs, one file per important domain. Proposed list, to validate:
  - [x] `wallet`
  - [ ] `dex`
  - [ ] `payments`
  - [ ] `oracles`
  - [ ] `identity`
  - [ ] `messaging`
  - [ ] `data-indexing`
  - [ ] `bridges`
- [ ] Contribution guidelines (`CONTRIBUTING.md`): how to add a card, guarantee-ID rules, voice and formatting rules, review flow.
- [ ] Primitive backlog from wallet.md: `light-clients` (W-4), `native-account-abstraction` (W-4), `transaction-simulation` (W-5).
- [ ] Wallet guarantee candidates, parked pending sharper framing: private transfers (must read as a goal layered on a transparent-by-default L1, never as a default), key custody and recovery (open question: one merged key-access-lifecycle guarantee or two).

### Milestone 3: publication

- [ ] Markdown lint: markdownlint config and CI job.
- [ ] Prose lint: Vale styles for glossary terminology consistency and marketing-language bans.
- [ ] Link check: lychee or equivalent CI action, internal and external links.
- [ ] Frontmatter and guarantee-ID validation: small CI step checking frontmatter fields, guarantee IDs resolving to a domain heading, no duplicate IDs, and `related`/`implements` slugs resolving to files.
- [ ] License.
- [ ] GitHub org and go-public timing.

## Open questions

- Domain taxonomy: app verticals (dex, payments, oracles) or the segments the team interfaces with (wallets, RPC/infra providers, L2s, dapps/SDKs, browsers/agents)? Neither list is settled.
- ~~Tooling scope~~ (resolved 2026-08-17): open source software an application can integrate (libraries, SDKs, reference implementations, self-hostable services); end-user apps only via their integrable parts; paid hosted plans acceptable when the documented integration path is fully open source and self-hostable.
- Specs scope: phase 1 likely references external specs only (permalinks pinned to commit or tag); specifying solutions here would be a phase 2 decision.
- Full primitive list beyond the initial three.
