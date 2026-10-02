---
name: stealth-addresses
last_reviewed: 2026-10-02
maturity: production
guarantees: [W-2]

related:
  requires: []
  composes_with: [pir, mixnets, shielded-pools]
  alternative_to: []
  see_also: []
---

# Stealth addresses

## What it is

An Ethereum address doubles as a public identity. Publishing one address to receive payments means every payment to it, past and future, is joined into a single visible history. Stealth addresses break that join: the sender derives a fresh one-time address for each payment, and only the recipient can compute the private key that controls it. On chain, the payments share nothing observable with each other or with the recipient's published identity.

The recipient publishes a stealth meta-address once: a pair of public keys, one for spending and one for viewing. From it, any sender can derive one-time addresses without further interaction. [ERC-5564](https://eips.ethereum.org/EIPS/eip-5564) standardizes the scheme for Ethereum (secp256k1 key agreement, view tags, a shared announcement contract) and [ERC-6538](https://eips.ethereum.org/EIPS/eip-6538) adds an on-chain registry for meta-addresses. Both are Final.

## What it guarantees

Satisfies W-2 (address-to-address unlinkability) on the receiving side: each incoming payment lands on an address derivable only from a secret shared between that sender and the recipient, so no on-chain observer can link the payments to each other or to the meta-address. The guarantee covers the chain's view; the recipient's own scanning traffic can leak the same link at the network layer (see Trust model).

## How it works

1. [recipient] Generates a spending keypair and a viewing keypair, and publishes the two public keys as a stealth meta-address, in the ERC-6538 registry or out of band.
2. [sender] Fetches the meta-address and generates a fresh ephemeral keypair for this payment.
3. [sender] Computes a shared secret from the ephemeral private key and the recipient's viewing public key, derives a 1-byte view tag from its hash, and derives the stealth address from the hash plus the recipient's spending public key.
4. [sender] Pays the stealth address and emits the ephemeral public key plus view tag through the singleton ERC-5564 announcer contract.
5. [wallet] Scans announcements, computing the shared secret for each with the viewing key; the view tag rules out 255 of 256 non-matching ones at that point, and full address derivation runs only for the remainder.
6. [recipient] Derives the stealth private key from the spending key and the shared secret, and spends.

## Trust model

The core scheme has no intermediary. The registry and announcer are permissionless singleton contracts that store and emit public data; neither can steal funds or deanonymize users. What must hold: the sender's software derives addresses correctly (a broken sender exposes only that sender's payment), and the recipient's viewing key stays secret (a leaked viewing key reveals which payments belong to the recipient, without exposing funds). Scanning is the soft spot: whichever endpoint serves the announcement log sees which announcements a wallet checks and from which IP, so scanning through a hosted RPC hands the W-2 link to the endpoint even though the chain never records it. This is the W-1 problem surfacing inside W-2.

## Known limits

- The funding-link problem: a fresh stealth address holds no gas, and funding its first spend from a known address re-creates the link the scheme just removed. Spending requires sponsored gas, gas paid in the received asset, or an unlinked funding source.
- Scanning costs fall on the recipient. View tags reduce the work per announcement, but announcements need not correspond to real payments and cost only ordinary transaction gas, so a spammer can inflate the log that every recipient must parse. Outsourcing the scanning hands the viewing key, and with it the full payment list, to the scanning service; oblivious message retrieval (see the [pir](pir.md) card) is the research path to delegating detection without that disclosure.
- Only the recipient link is hidden. Sender address, amount, asset, and timing remain public.
- Wallet-native support is minimal; the scheme runs today mostly in standalone applications, which limits the anonymity set.
- Post-quantum exposure is high. Announcements permanently publish ephemeral public keys on chain, so a future CRQC recovers the shared secrets retroactively and deanonymizes every past stealth payment; since spending public keys are published too, it also recovers spending keys, exposing stealth funds to theft. A [hybrid approach](https://ethresear.ch/t/pq-anonymity-for-stealth-address-protocol/26094) for the announcement side is documented, neither audited nor in production: an ML-KEM-768 encapsulation is combined with the ECDH secret, so the recipient link holds while either assumption holds. Costs: the meta-address grows from 66 to 1,250 bytes, the announcement from 34 to 1,122 bytes (69,300 gas measured against 28,313 classical), and scanning runs one ML-KEM decapsulation per announcement. Spending stays ECDSA; [post-quantum spending](https://ethresear.ch/t/pq-spending-for-stealth-address-protocol/26095) is research-stage.

## Implementations

[Umbra](https://github.com/ScopeLift/umbra-protocol) (its own pre-ERC contracts since 2021; an ERC-5564 version is pending) and [Fluidkey](https://github.com/fluidkey/fluidkey-stealth-account-kit) (ERC-5564) run stealth payments in production on mainnet and several L2s. Umbra's hosted frontend has been in maintenance mode since April 2026. Tooling card: [fluidkey-stealth-account-kit](../tooling/fluidkey-stealth-account-kit.md).

## Further reading

- [ERC-5564: Stealth Addresses](https://eips.ethereum.org/EIPS/eip-5564) (Final)
- [ERC-6538: Stealth Meta-Address Registry](https://eips.ethereum.org/EIPS/eip-6538) (Final)
- [An incomplete guide to stealth addresses](https://vitalik.eth.limo/general/2023/01/20/stealth.html), Vitalik Buterin, 2023
- [PQ anonymity for stealth address protocol](https://ethresear.ch/t/pq-anonymity-for-stealth-address-protocol/26094), namnc, 2026
- [PQ spending for stealth address protocol](https://ethresear.ch/t/pq-spending-for-stealth-address-protocol/26095), namnc, 2026
- Hybrid announcements: [reference implementation](https://github.com/namnc/pq-stealth-scheme3-public/tree/a08d5505b4545bbd61adf7aafc800032922380dc) (Rust, Apache-2.0), [browser demo](https://github.com/0xakk0r0kamui/pq-stealth-scheme3-demo/tree/aa0e3aaf62ba69c00d2404060347f3ebb5e835eb) (MIT), [Kohaku integration](https://github.com/ethereum/kohaku-rs/pull/29) (PR, under review)
