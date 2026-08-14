# Glossary

Vocabulary used across this repository.

## CROPS

The four properties this repository is organized around:

- **Censorship Resistance [CR]**: no actor can selectively exclude valid use or break functionality, including by gaining durable, non-competitive control of any critical mechanism.
- **Open Source and Free, as in Freedom [O]**: no privileged code or hidden specifications.
- **Privacy [P]**: user data is not exposed beyond necessity or against the user's interests.
- **Security [S]**: things do what they claim to do, no more and no less.

Guarantees are tagged with the bracketed letters of the properties they serve.

## Design vocabulary

- **Zero option**: for every affordance that has an intermediated path, an intermediary-free path that remains credible and accessible.
- **Chokepoint**: a position from which an actor can exclude, extract from, or impose rules on users.
- **Walkaway test**: a system passes if it continues to reliably function and evolve even if its original builders and stewards disappear. Applies to users too: no forced, complex migrations.
- **Points of leverage**: durable positions of control over users or infrastructure; between two credible designs, the one that removes them is preferred.

## Repository terms

- **Guarantee**: a desired property of one function of an application domain, stated descriptively and given a stable ID (for example W-1). Guarantees are testable properties, not recommendations.
- **Function**: one thing an application in a domain does (for a wallet: key management, network access, transaction broadcast, address management, discovery and safety features). Guarantees attach to functions.
- **Primitive**: a first-principles building block (mixnet, shielding, PIR, stealth addresses) that provides one or more guarantees, characterized by its trust model and maturity.
- **Maturity**: how deployable a primitive or tool is today. Levels: `research` (papers only, no usable implementation), `concept` (spec exists, no code), `testnet` (PoC or pilot implementations), `production` (audited implementations in sustained mainnet use).
- **State of the art**: a descriptive account of how the ecosystem currently handles a guarantee, written without naming specific applications.
