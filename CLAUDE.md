# Conventions

This repository doubles as LLM context. Every rule below exists to keep it reliable in that role.

## Files

- One concept per file. A file covers exactly one domain, primitive, tool, or spec.
- The folder is the type: `domains/`, `primitives/`, `tooling/`, `specs/`. There is no `type` frontmatter field.
- New files start from their folder's `_template.md`.

## Frontmatter

Shared fields (automated validation is planned; see [PRD.md](./PRD.md)):

```yaml
---
name: <kebab-case-slug, matches filename>
last_reviewed: YYYY-MM-DD
guarantees: [W-1, W-3]   # IDs satisfied (primitives, tooling, specs; not domains)
---
```

Primitives additionally carry `maturity`, `works-best-when` / `avoid-when`, `crops_profile` (badges) paired with `crops_context` (narratives on where each badge drifts in practice), `post_quantum` (risk / vector / mitigation), typed `related` cross-references, and optional `variants` for primitives whose flavors have diverging trust models. Tooling carries `maturity`, `implements` (primitive slug), and the works/avoid pair.

Maturity levels: `research` (papers only), `concept` (spec, no code), `testnet` (PoC or pilot), `production` (audited, sustained mainnet use).

## Guarantee IDs

- Domains declare guarantees as `### <ID>:` headings; the heading is the declaration. IDs are one uppercase letter per domain (W for wallet) plus a number.
- IDs are permanent. Never renumber. A withdrawn guarantee keeps its ID and is marked withdrawn.
- Primitives, tooling, and specs reference guarantees by ID in frontmatter and prose. This is the only cross-reference mechanism; do not restate guarantee text in other files.

## Cross-references

- Primitive-to-primitive links go in the `related` frontmatter block (`requires`, `composes_with`, `alternative_to`, `see_also`), as slugs matching `primitives/<slug>.md`.
- External references use permalinks pinned to a commit or tag, plus the EIP/ERC number where applicable.

## Voice

- Descriptive. State properties and facts. No RFC-2119 keywords (MUST, SHALL), no recommendations, no endorsements.
- Use terms as defined in [GLOSSARY.md](./GLOSSARY.md); do not invent synonyms.
- Never name specific applications when describing gaps or weaknesses. State of the art sections describe categories of behavior, not products.

## Formatting

- No em-dashes. Use commas, colons, or separate sentences.
- No contrastive constructions of the form "X is not Y, it's Z". Plain declaratives.
- Bold only for structural labels, never for emphasis in prose.

## Before pushing

Check manually until CI lands (see [PRD.md](./PRD.md)): frontmatter matches the template, guarantee IDs resolve to a domain heading, internal links resolve, terminology matches the glossary.
