---
name: oprf
last_reviewed: 2026-08-19
maturity: production
guarantees: [I-4, I-5]

related:
  requires: []
  composes_with: [anonymous-credentials, zk-group-membership]
  alternative_to: []
  see_also: []
---

# OPRF

## What it is

An oblivious pseudorandom function evaluates a server's keyed PRF on a client's blinded input: the client learns the output, the server learns neither the input nor the output. The verifiable variant (VOPRF) adds a proof that the advertised key was used. This one primitive underlies anonymous rate-limiting tokens, per-context pseudonyms issued blind, and password hardening.

## What it guarantees

I-4: spend an anonymous token where an app would otherwise demand identity; the token proves "was issued one" and nothing more. I-5: deterministic per-context identifiers (same input, same context, same pseudonym) that the issuer cannot link to inputs, because it never saw them.

## How it works

1. [user] Blinds the input with fresh randomness and sends the blinded value.
2. [operator] Evaluates the keyed PRF on the blinded value, learning nothing about the input.
3. [user] Unblinds the result, obtaining the PRF output.
4. [user] In the verifiable variant, checks the proof that the committed key was used.

## Trust model

The key is the trust locus. Rotation resets every derived pseudonym and voids outstanding tokens; leakage lets anyone mint valid outputs. The blinding hides inputs information-theoretically, so even the key holder never learns what it evaluated, past or future. Distributed t-of-n evaluation removes the single key holder at the cost of coordination.

## Known limits

- The server must be online for every evaluation: a liveness chokepoint even where it is privacy-blind, and the reason issuance usually happens in batches ahead of use.
- The OPRF itself needs rate limiting; it is the anti-abuse backstop for everything built on it.
- Verifiability only proves which key was used; key rotation policy is governance, not cryptography.
- Post-quantum: classical-group OPRFs lose unforgeability under a CRQC (mint at will); past blinding survives, being information-theoretic. Lattice-based OPRFs are research.

## Implementations

Specified in [RFC 9497](https://www.rfc-editor.org/rfc/rfc9497) (CFRG). Deployed in [Privacy Pass](https://github.com/cloudflare/privacypass-ts)'s privately verifiable tokens (the variant deployed at the largest scale uses blind RSA instead) and in password and backup hardening at messenger scale. Tooling cards pending.

## Further reading

- [RFC 9497: oblivious pseudorandom functions using prime-order groups](https://www.rfc-editor.org/rfc/rfc9497), IETF CFRG
- [RFC 9576: the Privacy Pass architecture](https://www.rfc-editor.org/rfc/rfc9576) and [RFC 9578: issuance protocols](https://www.rfc-editor.org/rfc/rfc9578), IETF
- [Privacy Pass: upgrading to the latest protocol version](https://blog.cloudflare.com/privacy-pass-standard/), Cloudflare
- [SoK: oblivious pseudorandom functions](https://eprint.iacr.org/2022/302), Casacuberta, Hesse, Lehmann, 2022
