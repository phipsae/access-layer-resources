---
name: semaphore
last_reviewed: 2026-08-19
maturity: production
implements: [zk-group-membership]
guarantees: [I-4, I-5]
---

# Semaphore

## What it implements

[zk-group-membership](../primitives/zk-group-membership.md) as contracts and JavaScript packages: anonymous group membership proofs with scoped nullifiers, proving in the browser. Group admission stays the integrator's design decision, which is where the primitive's trust concentrates.

## Integration guide

Prerequisite: a JavaScript app and a group admission rule.

1. [app] Install the `@semaphore-protocol` packages and deploy or reuse a group contract.
2. [user] Generates an identity; the app admits its commitment to the group.
3. [user] Proves membership and signals client-side, with the scope chosen per context.
4. [contract] Verifies and records the nullifier once per scope.

The [documentation](https://docs.semaphore.pse.dev/) is the guide.

## Audit and maturity status

v4 audited, trusted-setup ceremony completed 2024, sustained use (Zupass and others). MIT.

## Maintainer

PSE, at [semaphore-protocol/semaphore](https://github.com/semaphore-protocol/semaphore); listed in the [ethereum.org developer tools](https://ethereum.org/developers/tools/).
