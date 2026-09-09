---
name: verifiable-randomness
last_reviewed: 2026-09-09
maturity: production
guarantees: [O-3]

related:
  requires: []
  composes_with: []
  alternative_to: []
  see_also: [private-voting]
---

# Verifiable randomness

## What it is

Randomness a contract can consume together with a proof that the value was generated as claimed, rather than chosen by whoever delivered it. Two constructions cover the design space. Threshold beacons: a committee jointly signs each round with a threshold BLS key, and the signature is both unpredictable before the round and verifiable by anyone after it. VRF request-response schemes bind a random output to a specific request via a keyed proof that verifies on chain.

## What it guarantees

The randomness leg of O-3: a draw whose provenance a consumer verifies from public data. The proof verifies in the consuming contract, so no black-box pipeline sits between the randomness source and the application.

## How it works

The threshold-beacon flow:

1. [operator] Committee members each hold a share of a threshold BLS key, fixed at a distributed key generation ceremony.
2. [operator] Every round, members sign the round number; any threshold-sized subset assembles one deterministic group signature.
3. [node] The beacon publishes the signature; anyone verifies it against the group public key.
4. [contract] An on-chain verifier checks the signature (BN254 beacons verify via EVM pairing precompiles) and hashes it into the random value.
5. [contract] The application consumes the value, committed to a round chosen before the outcome was known.

VRF request-response replaces steps 1 to 3 with a single prover whose keyed proof verifies in step 4.

## Trust model

Threshold beacons trust the committee twice: fewer than the threshold colluding keeps outputs unpredictable, and enough honest members keep rounds flowing. The signature itself is deterministic, so a committee cannot grind outcomes; withholding a round is the residual bias vector. Oracle-push and request-response deployments add a delivery party whose liveness the application depends on, though a withheld delivery is publicly observable against the beacon's published rounds.

## Known limits

- Beacon withholding: a threshold coalition cannot forge a round but can decline to publish one it dislikes; applications mitigate by committing to a future round before it exists.
- Liveness is a dependency at the decisive moment: every construction waits on an external round or delivery.
- Stewardship concentrates: the main public beacon's operator diversity is the guarantee, and the company that stewarded its tooling shut down in February 2026, with maintenance continuing under the project's own stewards.
- Post-quantum exposure is high: BLS and elliptic-curve proofs are forgeable under a CRQC, which turns provable randomness into chooseable randomness. Migration needs hash-based or lattice-based proof schemes, at research stage for this primitive.

## Implementations

[drand](https://github.com/drand/drand) (threshold BLS beacon, dual Apache-2.0/MIT, operated by the League of Entropy since 2020; the evmnet beacon signs on BN254 every three seconds so verification runs on EVM precompiles). On-chain consumption: [Anyrand](https://github.com/frogworksio/anyrand) (MIT contracts, request-response over drand, audited October 2024, low commit activity since) and [Galxe drand-oracle](https://github.com/Galxe/drand-oracle) (MIT, oracle-push of drand rounds with an updater liveness dependency). Tooling cards pending.

## Further reading

- [drand documentation](https://docs.drand.love/), League of Entropy
- [Scalable bias-resistant distributed randomness](https://eprint.iacr.org/2016/1067), Syta et al., 2017
- [Verifiable random functions](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Pseudo%20Randomness/Verifiable_Random_Functions.pdf), Micali, Rabin, Vadhan, 1999
