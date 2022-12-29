---
title: "Zcash Ecosystem Security Overview"
date: 2022-12-28T11:00:00-07:00
slug: "overview"
draft: true
---

This page serves as an overview of the Zcash ecosystem from a security
auditor's point of view. It lists all of the projects that are intended to fall
under the scope of the [ZecSec project](https://forum.zcashcommunity.com/t/zcash-ecosystem-security-lead/42090), as well as past audit reports, notable
security bugs, and open security/privacy challenges in the Zcash ecosystem. You
can think of this page as "a security auditor's guide to Zcash."

This page is updated quarterly. The last update was on 2023-01-01.

{{< toc >}}

## ZecSec Audits

So far, the ZecSec project has completed the following security audits:

- Ywallet: *report forthcoming*.
- zecwallet-lite-cli and adityapk00's modifications to lightwalletd: *report forthcoming*.

## Highlighting Open Problems and Challenges

There are several "big picture" security and privacy challenges for Zcash that
are on ZecSec's radar. These are not necessarily being worked on as part of
ZecSec, but are being spotlighted here as our recommendations for ecosystem-wide
priorities.

### Scalable Privacy For Wallets

A challenge faced by all cryptocurrencies that aim to offer strong, formal
privacy guarantees is: how can wallets' find their funds and make their funds
spendable quickly and efficiently?

At present, Zcash uses "trial decryption", where the wallet must try to decrypt
every transaction on the blockchain to find the ones that belong to it. There are many
alternatives to this design with varying levels of privacy and scalability.
We've surveyed them in our post, [Scalable Private Money Needs Scalable Anonymous Messaging]({{<ref "scalable-private-money-needs-scalable-private-messaging.md">}}).

### Unintentional and/or Forced Use of Transparent Transactions

Usage of transparent transactions on the Zcash blockchain remains high.
Transparent addresses and transactions offer users the ability to transact
transparently, with consent, whenever they wish to do so. However, the high
transparent usage might be a sign that some users misunderstand the privacy
level provided by transparent transactions, or that users are forced into making
parts of their transactions transparent by third-parties who do not fully
support shielded addresses.

I know of at least one anecdote where someone put themselves at risk by using a
transparent address, because they thought "Zcash is private."

In our view, this problem should be tackled with (a) research into how
frequently users misunderstand the privacy properties of using transparent
addresses, (b) UX design within wallets that communicates privacy levels clearly
and simply, (c) support requests from the community to third parties and extra
engineering effort to increase shielded adoption, (d) an eventual removal of
transparent addresses, replaced by the use of viewing keys.

### Secure Messaging with the Memo Field

A popular use case for Zcash's memo field is sending messages. However, Zcash's
memo field currently lacks several properties that are required for secure
messaging. For example, it is not signed, so wallets cannot be sure of messages'
origins, and it is not forward-secure, so if keys are compromised all past
messages can be decrypted.

We would like to see the memo field extended with more features so that it is
easy to build messengers on top of it with Signal-like security properties.

### Mitigating C++ Memory Corruption Bug Risk in Zcashd

The main fullnode implementation of Zcash is written in C++, which puts it at
risk of entire classes of security vulnerabilities that cannot exist within
projects that are written in safer languages, like Rust. Deprecating the legacy
``zcashd`` codebase should be a priority, to be replaced by ``zebra``. These
risks could also be mitigated with better fuzzing of ``zcashd``'s code, but it's
probably better to get rid of the C++ code entirely.

### Easing Consensus Rule Security Review

A security engineer entering the Zcash ecosystem for the first time is faced
with the daunting task of *finding* the code that implements Zcash's consensus
rules. As a result, a large portion of audit time is used inefficiently, spent
finding and understanding consensus rule code.

The efficiency of future consensus rule audits can be improved by labeling the
locations of all consensus rules in the code. Electric Coin Co has already
[started on this project](https://github.com/zcash/zcash/pull/5912). Labeling
more consensus rules is on [ZecSec's 2023 roadmap]({{<ref
"zecsec-roadmap-for-2023.md">}}), but hopefully all maintainers of consensus
rule code will be able to contribute to this project.

## Projects In Scope (alphabetical order)

ZecSec aims to eventually provide security support for all Zcash-tangential
projects that are currently used, are in development, or run important
infrastructure for the community. Listed below are all of the projects that are
potentially in scope.

For more details on prioritization, see our [2023 roadmap]({{< ref "zecsec-roadmap-for-2023.md">}}). If your Zcash-related project is missing from the list below, [send us a note]({{< ref "contact.md">}}).

### Arti

TODO

### CoinPayments Zcash Integration

TODO

https://grants.zfnd.org/proposals/721236325-coinpayments-integration

### Electric Coin Co Projects

- lightwalletd -- has received no third-party security review
- Wallet App Threat Model

### ElectrumZ

TODO

### Elemental ZEC - Zcash UI Component Kit and Payment

TODO

https://grants.zfnd.org/proposals/648795356-elemental-zec-zcash-ui-component-kit-and-payment-processor

### Free2z

TODO

https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/24075557/

### FROST / multisig
	- TODO
### Ledger support for Transparent Zcash
	- See "Zondax" below for sheilded support.
### moeda.casa
	- TODO
	- https://grants.zfnd.org/proposals/1868138724-moeda-casa-smart-brazilian-fiat-to-crypto-over-zcash
### Nighthawk
	- Production lightwalletd instances (TODO: list the domains)
	- nighthawk-apps/zcash-explorer
		- and zcashblockexplorer.com -- production instance
	- nighthawk-apps/nighthawk-wallet-android -- Android wallet
	- nighthawk-apps/zcash-ios-wallet  -- iOS wallet
	- Left out a bunch of forks (unmodified) including but not limited to:
		- nighthawk-apps/bip39 (unmodified fork of a bip39 library)
		-  nighthawk-apps/zcash-android-wallet-sdk (safe to ignore, no modifications from upstream)
		- nighthawk-apps/lightwalletd
		- It's probably a good idea to periodically check these for having changes from upstream (note this list is incomplete)
### Oblivious Message Retreival (OMR)
	- TODO
### Payment Gateway with BTCPay
	- TODO
### Proof of Stake Design

TODO

### Quiet (formerly known as Zbay)

https://www.tryquiet.org/

TODO does this even use Zcash at all anymore? Doesn't look like it.

If it doesn't, take on some responsiblity for sec/performance marketing to bring it back.

### renZEC
	- TODO
	- https://grants.zfnd.org/proposals/118814097-bootstrapping-liquidity-for-renzec-on-binance-smart-chain

### Stagnum (Zcash Node Hardware)
	- "Zcash Node - RFP for portable device for running Zcashd nodes"
	- TODO: find repos
	- https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/31829236/

### Telegram Anti-Scam Bot
	- TODO
	- https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/30972940/

### Thorchain Integration
	- TODO

### Trezor Support for Transparent Zcash

TODO

### Trezor Support for Shielded Zcash

TODO
https://grants.zfnd.org/proposals/1792958360-trezor-support-for-zcash-shielded-transactions

Master PR:
https://github.com/trezor/trezor-firmware/pull/2472

### Ywallet
- Orchard impl (forthcoming in a funded grant)
- UA impl
- He also wrote a cold wallet: https://grants.zfnd.org/proposals/643943131-cold-wallet
  - Maybe break this out into its own item?

### Zboard
- TODO
- https://grants.zfnd.org/proposals/1854600060-zboard-global-decentralized-news-hub

### Zcash Blockchain Infrastructure Grant
- TODO

### Zcash Community Forum
- https://forum.zcashcommunity.com/
- Deserves an audit, may be used to discus vulnerability information, etc.

### Zcash Foundation Projects
- TODO

### Zcash Media
- Just watch their content and ensure technical / privacy accuracy

### Zcash Observatory
- https://grants.zfnd.org/proposals/21786689-zcash-observatory

### Zcash Protocol Specification
- TODO

### ZECPages (and Michael Harms' other repos)
- michaelharms6010/zecpages
- Its production lightwalletd server at https://lightwalletd.zecpages.com:443
  - It invites external connections on their webpage
- (he has other zcash related repos but says they aren't worth including)
- Testnet Faucet - https://grants.zfnd.org/proposals/2008792221-zecpages-testnet-faucet-app-development-1-year-infra

### ZecWallet
- adityapk00/* TODO
- zecwalletco/zecwallet-mobile TODO WHAT IS THIS??
- Production lightwalletd instances (TODO: list the domains)
- TODO: find/audit their orchard impl (recent grant)
- There are many many individual grants, if you're trying to list them all!
- Orchard Grant: https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/26631279/

### ZecWear - merch - https://zecwear.com/
- TODO

### ZEGA - "A command line encrypted-file-sharing CLI using ZecWallet light client."
- https://github.com/wh00hw/ZEGA

### Zeme Teme
- https://zeme.team

### Zemo

https://forum.zcashcommunity.com/t/zemo-your-web3-inbox/41063/52

### Zephyr (Metamask-like browser extension)
- https://forum.zcashcommunity.com/t/project-zephyr-update/40657/6
- TODO

### ZGo (Point of Sale) https://zgo.cash/ https://twitter.com/ZGoCashApp
- TODO where is their code? I couldn't find their github
- https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/25974825/

### Ziggurat
- TODO: list repos
- TODO: read up on all grant proposals
- Latest one includes red teaming on testnet: https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/33263916/
- 2.0: https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/23814137/

### ZingoLabs' wallet (zingolabs on github):
- zingolib (library and CLI interface, similar to zecwallet-lite-cli)
- zingo-mobile (the moble apps proper)
- zingo (a fork of zecwallet-lite ?)
- Their orchard implementation: https://forum.zcashcommunity.com/t/implement-orchard/42706
- TODO: see other grants for better understanding
- Orchard Rust Library and user-facing library
  - https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/32468187/

### ZIPs
	- TODO: various community-authored zips

### Zondax (Zcash Ledger App)
- zondax/ledger-zcash -- the main ledger app
- zondax/zindexer -- no idea what this is but it has a "z"
- zondax/ledger-zcash-rs -- rust library used by the ledger app
- zondax/zxtestrunner -- no idea but it has a "z"
- TODO: find the PRs they have submitted to various repos for integration
- https://grants.zfnd.org/proposals/310598051-new-zcash-ledger-app-integration

### ZSAs -- by QEDIT
- Everything is linked here: https://forum.zcashcommunity.com/t/grant-update-zcash-shielded-assets-monthly-updates/41153
- TODO break out all the links to ZIP PRs
- https://zcashgrants.org/gallery/25215916-53ea-4041-a3b2-6d00c487917d/33106640/

## List of Security Audits

### 2016

The initial implementation of Zcash was audited by NCC Group and Coinspect. The audits
were announced [here](https://electriccoin.co/blog/auditing-zcash/), the
completed reports are linked
[here](https://electriccoin.co/blog/audit-results/), and some discussion of the
issue mitigations is
[here](https://electriccoin.co/blog/new-beta-release-audit-mitigations-and-bug-fixes/).

Zcash's first parameter generation ceremony was also [audited by NCC Group](https://electriccoin.co/blog/ceremony-audit-results/).

### 2017

N/A

### 2018

In 2018, audits were performed by Least Authority, Coinspect, NCC Group, 
Kudelski Security, and QEDIT. The audit results are discussed
[here](https://electriccoin.co/blog/2018-security-audit-results-in-detail/), but
unfortunately the links to many of the audit reports are now broken.

Least Authority's audit reports are available here: [summary](https://medium.com/least-authority/zcash-security-audit-a72945842f8e), [Overwinter audit](https://leastauthority.com/wp-content/uploads/2019/08/LeastAuthority-Zcash-Implementation-Analysis-and-Overwinter-Specification.pdf), [Overwinter+Sapling audit](https://leastauthority.com/static/publications/LeastAuthority-Zcash-Overwinter+Sapling-Specification-Final-Audit-Report.pdf), [Sapling RPC audit](https://leastauthority.com/static/publications/LeastAuthority-Zcash-Sapling-Implementation-RPC-Interface-Updated-Audit-Report.pdf).

QEDIT's audit of Sapling is available [here](https://raw.githubusercontent.com/QED-it/sapling-audit/master/sapling-audit-report.pdf).

### 2019

In 2019, [TrailOfBits audited ZecWallet](https://github.com/trailofbits/publications/blob/master/reviews/zecwallet.pdf) and [Zcash Blossom was audited by Coinspect and NCC Group](https://electriccoin.co/blog/blossom-network-upgrade-and-wallet-security-audits/).

### 2020

In 2020, [TrailOfBits audited Zcash Heartwood](https://electriccoin.co/blog/heartwood-security-assessment-turns-up-no-major-issues/), [NCC Group audited Zcash Canopy](https://electriccoin.co/blog/canopy-security-assessment-complete/), and [Electric Coin Co performed an internal audit of their wallets](https://electriccoin.co/blog/ecc-wallet-threat-model-and-security-assessment-results/).

### 2021

In 2021, [NU5 was audited by NCC Group and QEDIT](https://electriccoin.co/blog/nu5-security-assessments-complete/).

### 2022

In 2022, [the design of the Halo2 proving system was audited by Mary Maller](https://z.cash/halo2-audit/).

ZecSec performed an audit of Ywallet (*report forthcoming*) and zecwallet-lite-cli (*report forthcoming*).

## Notable Security Bugs and Other Security Research

Secure systems can only be built by learning from mistakes. Here is a list of
notable bugs and other security research that we can learn from to make Zcash a
more robust platform.

### Viewing key disclosure bug

The ECC wallets and Nighthawk accidentally leaked the wallet's viewing key where
the reply address should been within the "Reply-To" component of the memo field.

[Disclosure Blog Post](https://electriccoin.co/blog/privacy-leak-bug-discovered-in-nighthawk-and-ecc-wallets/)

### Counterfeiting vulnerability

A mistake in the BCTV14 paper, which carried over to its implementation, made it
possible to violate the soundness property of the zero-knowledge proving system
originally used by Zcash. If this bug was exploited, it would have allowed for
the forgery of Zcash funds, limited by the [turnstile
design](https://electriccoin.co/blog/turnstile-enforcement-against-counterfeiting/)
that was built as a defense-in-depth measure against these kinds of attacks.

This bug was assigned CVE-2019-7167.

[Disclosure Blog Post](https://electriccoin.co/blog/zcash-counterfeiting-vulnerability-successfully-remediated/)

### Side-Channel Attacks on Zcash Node Transaction Receivers

In an [academic paper](https://eprint.iacr.org/2020/220), Florian Tramèr, Dan
Boneh, and Kenneth G. Paterson explored the use of timing, traffic analysis, and
error-message side-channels to de-anonymize nodes in receipt of transactions.

They found that when a Zcash node successfully decrypted a transaction, but the
note commitment check failed, the node would send an error message back to the
attacker, indicating to the attacker that the node had the key required to
decrypt the transaction. Additionally, since the P2P message processing all
happened on the same thread, an attacker could measure the response time of a
"ping" message to determine if the note commitment check was happening; another
way to confirm the node had the correct decryption key. Through either of these
approaches, it was possible for an attacker to conclude that a known shielded
address belonged to the victim node.

The vulnerability in Zcash was assigned CVE-2019-16930, and there is a
corresponding [security
announcement](https://z.cash/support/security/announcements/security-announcement-2019-09-24/).

### Sapling Woodchipper

"[Sapling Woodchipper](https://saplingwoodchipper.github.io/)" is a name given
to the concept that as long as transaction fees are cheap, it is inexpensive to
fill up blocks on the Zcash blockchain. It was assigned CVE-2019-1636 by its
initial reporter.  It is currently being mitigated by the rollout of a new
transaction fee structure, specified in [ZIP-317](https://zips.z.cash/zip-0317).

### InternalH Collision Vulnerability

In the original [Zerocash paper](https://eprint.iacr.org/2014/349) that Zcash
was based on, a 128-bit hash was used to build a commitment scheme. This was
vulnerable to a birthday attack, which would have made it possible to
counterfeit funds. The bug was [identified and fixed prior to Zcash's
launch](https://electriccoin.co/blog/fixing-zcash-vulns/).

### ZecWallet-Lite TLS Authentication Bug

Sarah Jamie Lewis [discovered that a previous version of ZecWallet-Lite was not
properly authenticating TLS
connections](https://twitter.com/SarahJamieLewis/status/1236137690900258818), so
that its communications with the lightwalletd server could be intercepted by an
attacker.

### ZecWallet Nonce Reuse Replay Attack

ZecWallet [optionally uses a "wormhole" service to allow a companion smartphone app to connect to the user's full node](https://docs.zecwallet.co/android/). The encryption used by the wormhole protocol was vulnerable to a replay attack that allowed for nonce reuse. This was [reported by Sarah Jamie Lewis](https://twitter.com/SarahJamieLewis/status/1234585483482550273?s=20). See also [a further bug in the fix to the issue](https://github.com/ZcashFoundation/zecwallet/issues/243).

### Fuzzing ``zcashd`` with Kubernetes

Electric Coin Co engaged in a project to [fuzz ``zcashd``'s C++ codebase in an
automated way using
Kubernetes](https://electriccoin.co/blog/fuzzing-zcash-with-kubernetes/).

### Miscellanious Bugs & Security Announcements

- https://electriccoin.co/blog/nu5-activation-and-halo-arc-release-delayed-for-remediation-of-consensus-bug/
- https://electriccoin.co/blog/security-announcement-2017-04-13/
- https://electriccoin.co/blog/security-announcement-2017-04-12/
- https://electriccoin.co/blog/security-announcement-2016-11-22/


### Academic Papers Analyzing Zcash

Zcash has been investigated in several academic works, linked below:

- [Privacy Aspects and Subliminal Channels in Zcash](https://dl.acm.org/doi/abs/10.1145/3319535.3345663)
- [Privacy and Linkability of Mining in Zcash](https://ieeexplore.ieee.org/abstract/document/8802711)
- [Blockchain Access Privacy: Challenges and Directions](https://ieeexplore.ieee.org/abstract/document/8425613)
- [Security and privacy of mobile wallet users in Bitcoin, Dash, Monero, and Zcash](https://www.sciencedirect.com/science/article/abs/pii/S1574119218307181)
- [A Look into Privacy-Preserving Blockchains](https://ieeexplore.ieee.org/abstract/document/9035235)
- [On the linkability of Zcash transactions](https://arxiv.org/abs/1712.01210) ([Electric Coin Co's response](https://electriccoin.co/blog/new-research-on-shielded-ecosystem/))
- [An Empirical Analysis of Anonymity in Zcash](https://www.usenix.org/conference/usenixsecurity18/presentation/kappos&lang=en)
- [Extending the Anonymity of Zcash](https://arxiv.org/abs/1902.07337)
- [Deanonymization and Linkability of Cryptocurrency Transactions Based on Network Analysis](https://ieeexplore.ieee.org/abstract/document/8806723)
- [Map-Z: Exposing the Zcash Network in Times of Transition](https://ieeexplore.ieee.org/abstract/document/8990796)
- [A Refined Analysis of Zcash Anonymity](https://ieeexplore.ieee.org/abstract/document/8993834)
- [Attacking Zcash For Fun and Profit](https://eprint.iacr.org/2020/627)
- [Anonymity Analysis of Bitcoin, Zcash, and Ethereum](https://ieeexplore.ieee.org/abstract/document/9389894)
- [An Analysis of Anonymity in the Zcash Cryptocurrency](https://deepblue.lib.umich.edu/handle/2027.42/143130)
- [Modeling the Block Verification Time of Zcash](https://ieeexplore.ieee.org/abstract/document/9583684)
- [A Comparative Study of Privacy-Preserving Cryptocurrencies: Monero and ZCash](http://www.dgalindo.es/mscprojects/sofie.pdf)
- [Exploring the use of Zcash cryptocurrency for illicit or criminal purposes](https://www.rand.org/pubs/research_reports/RR4418.html)
- [PING and REJECT: The Impact of Side-Channels on Zcash Privacy](https://crypto.stanford.edu/timings/pingreject.pdf)
- [A Review of Zcash as a Cryptocurrency Platform Aimed Towards Maintaining Privacy Between All Parties](https://static1.squarespace.com/static/553d790de4b08ceb08ab88fd/t/60022ff3652b4632bc8d632b/1610756084528/Zcash_SciPaper.pdf)

If you know of Zcash-related security research that's missing from this list, please [send us a note]({{<ref "contact.md">}}).