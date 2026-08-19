---
name: fluidkey-stealth-account-kit
last_reviewed: 2026-08-19
maturity: production
implements: [stealth-addresses]
guarantees: [W-2, P-1]
---

# Fluidkey stealth account kit

## What it implements

The core cryptographic functions behind Fluidkey's production [stealth-addresses](../primitives/stealth-addresses.md) flows: key generation from a signature, stealth account derivation, and Safe address prediction, as a TypeScript library. Recovery scans by deterministic key derivation rather than announcements; gas funding stays the integrator's problem, per the primitive's known limits.

## Integration guide

Prerequisite: a TypeScript app handling keys.

1. [app] Install `@fluidkey/stealth-account-kit`.
2. [app] Generate the user's spending and viewing keys from a signature.
3. [app] Derive stealth accounts for receiving; recover them by re-deriving ephemeral keys deterministically.

The [repository](https://github.com/fluidkey/fluidkey-stealth-account-kit) is the guide.

## Audit and maturity status

Audited by Dedaub (2024); powers Fluidkey's production payments on mainnet and several L2s. MIT.

## Maintainer

Fluidkey, at [fluidkey/fluidkey-stealth-account-kit](https://github.com/fluidkey/fluidkey-stealth-account-kit).
