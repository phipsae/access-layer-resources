---
name: interfold
last_reviewed: 2026-09-09
maturity: usable
implements: [private-voting, private-shared-state]
guarantees: [G-1]
---

# The Interfold

## What it implements

Committee-class computation over encrypted inputs, the FHE flavor of [private-shared-state](../primitives/private-shared-state.md): a network of ciphernodes coordinates Encrypted Execution Environments (E3s) where a threshold committee computes on ciphertexts nobody individually can decrypt, with input validity proven in zero knowledge (Noir circuits). Applied to voting it carries the committee model of [private-voting](../primitives/private-voting.md) (G-1): the CRISP protocol tallies encrypted ballots homomorphically, and an Aragon integration of CRISP was announced in June 2026. Formerly named Enclave.

## Integration guide

Prerequisite: a contract stack and the Interfold SDK.

1. [app] Follow the quick start: install the SDK and scaffold from a project template.
2. [app] Write the E3 program contract and the Noir circuit for input validity.
3. [app] Request an E3; a ciphernode committee forms, computes, and publishes the threshold-decrypted result.
4. [app] Study [CRISP](https://github.com/gnosisguild/CRISP) as the reference E3 application.

The [documentation](https://docs.theinterfold.com/) is the guide.

## Audit and maturity status

Testnet stage: the documented deployment path targets Sepolia, and no audit is published. Active development (Gnosis Guild lineage). LGPL-3.0 for the protocol monorepo; CRISP is GPL-3.0.

## Maintainer

The Interfold project (formerly Gnosis Guild's Enclave), at [theinterfold/interfold](https://github.com/theinterfold/interfold), docs at [docs.theinterfold.com](https://docs.theinterfold.com/).
