# Domains

Find the guarantee for what you want to build, then open its domain page.

| I want to... | Guarantee | Domain | Next step |
| --- | --- | --- | --- |
| let users query chain data without the server learning what they look at | X-1 | [data-indexing](./data-indexing.md#x-1-private-reads-p) | none yet |
| let users check that the data an indexer returns is correct | X-2 | [data-indexing](./data-indexing.md#x-2-verifiable-answers-s) | none yet |
| let anyone rebuild my dataset from public data | X-3 | [data-indexing](./data-indexing.md#x-3-reproducible-datasets-cr-o) | none yet |
| show filtered chain data without hiding that it was filtered | X-4 | [data-indexing](./data-indexing.md#x-4-transparent-filtering-cr-s) | none yet |
| stop others from seeing and front-running a user's order | D-1 | [defi](./defi.md#d-1-pre-execution-order-privacy-p) | [shutter-rpc](../tooling/shutter-rpc.md) |
| let users hold DeFi positions without publishing balances or strategy | D-2 | [defi](./defi.md#d-2-position-privacy-as-an-explicit-layer-p) | [kohaku](../tooling/kohaku.md) |
| keep every action and exit usable if my frontend goes down | D-3 | [defi](./defi.md#d-3-zero-option-on-access-and-exit-cr) | none yet |
| make every admin power public and delayed so users can leave first | D-4 | [defi](./defi.md#d-4-no-hidden-admin-switches-s-o) | none yet |
| keep votes secret even if a voter is pressured or paid | G-1 | [governance](./governance.md#g-1-coercion-resistant-ballots-p) | [interfold](../tooling/interfold.md), [maci](../tooling/maci.md) |
| let anyone put up a proposal without someone approving it | G-2 | [governance](./governance.md#g-2-permissionless-proposals-cr) | none yet |
| make sure no one can drop votes or stall the count | G-3 | [governance](./governance.md#g-3-censorship-resistant-tallying-cr-s) | [maci](../tooling/maci.md) (coordinator can stall the count) |
| make a passed vote execute on chain exactly as voted | G-4 | [governance](./governance.md#g-4-votes-bind-execution-s) | none yet |
| let members delegate votes and take them back at any time | G-5 | [governance](./governance.md#g-5-revocable-scoped-delegation-s) | none yet |
| check a fact about a user (over 18, member) without seeing who they are | I-1 | [identity](./identity.md#i-1-minimal-disclosure-by-construction-p) | [zk-proof-of-personhood](../tooling/zk-proof-of-personhood.md) ([spec 5](https://github.com/ethereum/access-layer-specs/blob/59c6310660edcadf172158bc8a1955be1dfad8ab/specs/5-zk-proof-of-personhood/README.md)) |
| accept credentials from more than one issuer so no one can lock users out | I-2 | [identity](./identity.md#i-2-no-issuer-chokepoint-cr) | none yet |
| let users make proofs on their own device without a server | I-3 | [identity](./identity.md#i-3-local-non-custodial-proving-s-cr) | [zk-proof-of-personhood](../tooling/zk-proof-of-personhood.md) ([spec 5](https://github.com/ethereum/access-layer-specs/blob/59c6310660edcadf172158bc8a1955be1dfad8ab/specs/5-zk-proof-of-personhood/README.md)) |
| allow one vote or post per member of a group, without names | I-4 | [identity](./identity.md#i-4-sybil-resistance-without-identity-p-cr) | [semaphore](../tooling/semaphore.md) |
| let a user prove things many times without the proofs being linked | I-5 | [identity](./identity.md#i-5-unlinkable-reuse-p) | [semaphore](../tooling/semaphore.md) |
| get price data that one source or reporter can't push around | O-1 | [oracle](./oracle.md#o-1-manipulation-resistant-values-s) | none yet |
| keep an oracle feed running if its operator stops | O-2 | [oracle](./oracle.md#o-2-permissionless-reporting-and-recovery-cr) | none yet |
| prove where an oracle value came from and how it was computed | O-3 | [oracle](./oracle.md#o-3-verifiable-provenance-o-s) | [tlsnotary](../tooling/tlsnotary.md) |
| let users prove a fact from a web account without sharing their login | O-4 | [oracle](./oracle.md#o-4-private-user-data-attestation-p) | [tlsnotary](../tooling/tlsnotary.md) |
| let users pay without showing their balance or who they pay | P-1 | [payments](./payments.md#p-1-counterparty-privacy-on-a-transparent-ledger-p) | [fluidkey-stealth-account-kit](../tooling/fluidkey-stealth-account-kit.md) (partial, hides only the recipient, sender and amount stay public), [kohaku](../tooling/kohaku.md) |
| make sure a payment still works if the relayer or payment API refuses it | P-2 | [payments](./payments.md#p-2-no-intermediary-can-block-a-payment-cr) | none yet |
| set up recurring payments the user can limit and cancel alone | P-3 | [payments](./payments.md#p-3-bounded-recurring-authority-s) | none yet |
| let the payee confirm a payment is final from chain data alone | P-4 | [payments](./payments.md#p-4-independently-verifiable-settlement-s-cr) | none yet |
| let users get transactions into my rollup if the sequencer ignores them | R-1 | [rollup](./rollup.md#r-1-inclusion-without-the-sequencer-cr) | none yet |
| let users withdraw to L1 if the rollup operator disappears | R-2 | [rollup](./rollup.md#r-2-exit-without-the-operator-cr-s) | none yet |
| have rollup state checked by proofs, not by the operator's word | R-3 | [rollup](./rollup.md#r-3-trustless-state-updates-s) | none yet |
| publish enough data that anyone can rebuild rollup state | R-4 | [rollup](./rollup.md#r-4-data-availability-for-permissionless-reconstruction-cr-s) | none yet |
| run a rollup with private execution that still exits to Ethereum | R-5 | [rollup](./rollup.md#r-5-private-execution-as-a-rollup-level-option-p) | none yet |
| hide the user's IP from the RPC provider | W-1 | [wallet](./wallet.md#w-1-address-to-ip-unlinkability-p) | [anon-rpc](../tooling/anon-rpc.md) |
| stop a user's addresses from being linked to each other | W-2 | [wallet](./wallet.md#w-2-address-to-address-unlinkability-p) | [fluidkey-stealth-account-kit](../tooling/fluidkey-stealth-account-kit.md) |
| keep wallet usage data on the device unless the user agrees | W-3 | [wallet](./wallet.md#w-3-no-usage-data-exfiltration-p-s) | none yet |
| keep a way to send transactions without our RPC, relayer or paymaster | W-4 | [wallet](./wallet.md#w-4-zero-option-on-every-intermediated-path-cr) | [kohaku](../tooling/kohaku.md) |
| let users inspect and override warnings, filters and simulations | W-5 | [wallet](./wallet.md#w-5-user-controlled-safety-features-s-o) | [kohaku](../tooling/kohaku.md) |
| let users recover or rotate keys without a custodian | W-6 | [wallet](./wallet.md#w-6-key-lifecycle-without-custody-or-linkage-s-cr-p) | none yet |
