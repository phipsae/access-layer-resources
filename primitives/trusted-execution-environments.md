---
name: trusted-execution-environments
last_reviewed: 2026-08-27
maturity: production
guarantees: [W-1, X-1]

related:
  requires: []
  composes_with: [mixnets]
  alternative_to: [pir]
  see_also: [encrypted-mempools, private-shared-state]
---

# Trusted execution environments (TEEs)

## What it is

A trusted execution environment is a hardware-isolated region of a processor that runs code the host machine cannot inspect or alter, paired with remote attestation: a vendor-rooted signature proving to a remote party exactly which code is running inside. The operator of the machine keeps the power to switch the service off and loses the power to read or modify what happens inside it.

For Ethereum reads, the deployed shape is the TEE relay: an RPC proxy whose request handling runs inside an enclave, so queried addresses, request contents, and originating metadata stay unreadable to the relay operator. This closes the query-content leak with hardware trust while cryptographic reads over `pir` mature toward deployment.

## What it guarantees

Satisfies the content side of W-1 and X-1 under a hardware trust assumption: the endpoint operator serves queries it cannot read, because reading them would require breaking the enclave rather than inspecting a process. Attestation lets the wallet check, before sending anything, that the relay runs the published code and no other.

## How it works

1. [operator] Runs the relay inside an enclave and publishes the expected code measurement.
2. [wallet] Requests an attestation and verifies the vendor-rooted signature against the published measurement.
3. [wallet] Establishes an encrypted channel that terminates inside the enclave, then sends queries over it.
4. [operator] The enclave fetches chain data, answers the query, and discards the request state; the host sees only ciphertext.
5. [wallet] Receives the response over the same channel.

## Trust model

Three parties must hold: the chip vendor (the attestation root and the silicon itself), the attestation infrastructure that distributes and revokes keys, and the enclave's resistance to side channels on shared hardware. A broken enclave fails silently: queries keep flowing and confidentiality is gone with nothing observable on the wire, whereas a cryptographic scheme keeps its guarantee until the underlying assumption falls publicly. Availability stays with the operator, who can withhold service at will.

## Known limits

- The side-channel record is long: cache timing, speculative execution, and voltage attacks have each broken production enclaves (SGX generations retired for exactly this), and patches arrive after publication, never before.
- The attestation root is a chokepoint: one vendor signs the fleet, and a revocation or licensing decision by that vendor is a service shutdown for everyone downstream.
- Verifying attestation from a browser or wallet context is its own integration problem; skipping it silently reduces the relay to an ordinary trusted proxy.
- The relay still observes traffic volume and timing from each connection; `mixnets` cover that remainder.
- Post-quantum exposure sits in the channel and the attestation signatures, both classical today: recorded enclave traffic is harvestable for later decryption, and a CRQC could forge attestations. Migration follows the vendors' signature schemes.

## Implementations

Open source components: [Gramine](https://github.com/gramineproject/gramine) (library OS for running unmodified code in SGX enclaves) and [Automata's DCAP attestation contracts](https://github.com/automata-network/automata-dcap-attestation) (on-chain attestation verification). Hosted TEE-attested RPC relays serve Ethereum and dozens of other networks in production; their relay code is not open source, so none is named here. Tooling cards pending.

## Further reading

- [Intel SGX explained](https://eprint.iacr.org/2016/086), Costan and Devadas, 2016
- [A maximally simple L1 privacy roadmap](https://ethereum-magicians.org/t/a-maximally-simple-l1-privacy-roadmap/23459), Buterin, 2025; TEE relays as the interim read-privacy step
- [SGX.fail](https://sgx.fail/), a catalog of production SGX attacks
