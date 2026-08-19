---
name: shutter-rpc
last_reviewed: 2026-08-19
maturity: usable
implements: [encrypted-mempools]
guarantees: [D-1]
---

# Shutter RPC

## What it implements

The opt-in path into the only live threshold [encrypted mempool](../primitives/encrypted-mempools.md): transactions sent through the Shutter endpoint on Gnosis Chain encrypt to the keyper committee and decrypt only after ordering commits.

## Integration guide

Prerequisite: the flow runs on Gnosis Chain.

1. [app] Point the wallet or app RPC at the Shutter endpoint; transactions encrypt transparently.
2. [user] Signs as usual; the envelope carries the ciphertext.
3. [node] Decryption and execution follow the committed order.

The [documentation](https://docs.shutter.network/) is the guide.

## Audit and maturity status

Live opt-in on Gnosis Chain since 2024, beta; the Ethereum path through PBS and mev-commit preconfirmations is pending. MIT.

## Maintainer

Shutter Network, at [shutter-network/rolling-shutter](https://github.com/shutter-network/rolling-shutter).
