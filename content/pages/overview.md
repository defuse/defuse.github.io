---
title: "Zcash Ecosystem Security Overview"
date: 2022-12-28T11:00:00-07:00
slug: "overview"
---

This page serves as an overview of the Zcash ecosystem from a security
auditor's point of view. It lists all of the projects that are intended to fall
under the scope of the ZecSec project, as well as past audit reports, notable
security bugs, and open security/privacy challenges in the Zcash ecosystem. You
can think of this page as "a security auditor's guide to Zcash."

This page is updated quarterly. The last update was on 2023-01-01.

## ZecSec Audits

So far, the ZecSec project has completed the following security audits:

- Ywallet: report forthcoming.
- zecwallet-lite-cli and adityapk00's modifications to lightwalletd: report forthcoming.

## Open Problems and Challenges

There are several "big picture" security and privacy challenges for Zcash that
are on ZecSec's radar.

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

In our view, this problem should be tackled with (a) research into how
frequently users misunderstand the privacy properties of using transparent
addresses, (b) UX design within wallets that communicates privacy levels clearly
and simply, and (c) community pushes and more engineering effort to increase
shielded support.

### Secure Messaging with the Memo Field

A popular use case for Zcash's memo field is sending messages. However, Zcash's
memo field currently lacks several properties that are required for secure
messaging. For example, it is not signed, so wallets cannot be sure of messages'
origins, and it is not forward-secure, so if keys are compromised all past
messages can be decrypted.

We would like to see the memo field extended with more features so that it is
easy to build messengers on top of it with Signal-like security properties.

### Vulnerability Disclosure Coordination & Standards

TODO

https://github.com/zcash/zcash/blob/master/SECURITY.md#bilateral-responsible-disclosure-agreements

need to be expanded e.g. if we ever need a coordinated release of wallet updates

### Mitigating C++ Memory Corruption Bug Risk in Zcashd

The main fullnode implementation of Zcash is written in C++, which puts it at
risk of entire classes of security vulnerabilities that cannot exist within
projects that are written in safer languages, like Rust. Deprecating the legacy
``zcashd`` codebase should be a priority, to be replaced by ``zebra``. These
risks could also be mitigated with better fuzzing of ``zcashd``'s code.

### Easing Consensus Rule Security Review

TODO

## Projects In Scope (alphabetical order)

The list below includes all Zcash-tangential projects that are either currently
used, in development, or are important infrastructure for the Zcash community.
Time permitting, these are the projects that should receive an audit or other
kinds of security support from ZecSec.

For more details on prioritization, see our [2023 roadmap]({{< ref "zecsec-roadmap-for-2023.md">}}).

If your Zcash-related project is missing from the list below, [send us a
note]({{< ref "contact.md">}}).

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

- NCC and CoinSpect and Solar Designer audits of initial Zcash implementation
  - https://electriccoin.co/blog/auditing-zcash/
  - https://electriccoin.co/blog/audit-results/
    - Mitigations (?) https://electriccoin.co/blog/new-beta-release-audit-mitigations-and-bug-fixes/

### 2017

	- Audit of the setup ceremony
		- https://electriccoin.co/blog/ceremony-audit-results/

### 2018


	- https://leastauthority.com/wp-content/uploads/2019/08/LeastAuthority-Zcash-Implementation-Analysis-and-Overwinter-Specification.pdf
	- 2018 Security Audits by TODO
		- https://electriccoin.co/blog/2018-security-audit-results-overview/
		- https://electriccoin.co/blog/2018-security-audit-results-in-detail/
		- https://electriccoin.co/blog/2018-security-audits/
		- https://electriccoin.co/blog/2018-zcash-security-audit-details/

### 2019
	- ZecWallet audit by TrailOfBits
		- https://github.com/trailofbits/publications/blob/master/reviews/zecwallet.pdf
	- Blossom audits by Coinspect and NCC Group
		- https://electriccoin.co/blog/blossom-network-upgrade-and-wallet-security-audits/
### 2020

- TrailOfBits Audit of Heartwood
  - https://electriccoin.co/blog/heartwood-security-assessment-turns-up-no-major-issues/
- NCC Audit of Canopy
  - https://electriccoin.co/blog/canopy-security-assessment-complete/
- Internal ECC Wallet audits
  - https://electriccoin.co/blog/ecc-wallet-threat-model-and-security-assessment-results/

### 2021
	- NU5 audits by NCC and QEDIT
		- https://electriccoin.co/blog/nu5-security-assessments-complete/

### 2022
	- Halo2 audit by Mary Maller
		- https://z.cash/halo2-audit/
	- ZecSec Ywallet audit (TBD)
	- ZecSec zecwallet-lite-cli audit (TBD)

## Notable Past Security Bugs and Other Security Research

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

### Error Message Side-Channel Revealing Ownership of Shielded Addresses
	- CVE-2019-16930
	- https://z.cash/support/security/announcements/security-announcement-2019-09-24/
	- TODO: I think there was an academic paper about this?

### Sapling Woodchipper

### InternalH Collision Vulnerability (pre-launch)

	- https://electriccoin.co/blog/fixing-zcash-vulns/

### Miscellanious Bugs & Security Announcements

- https://electriccoin.co/blog/nu5-activation-and-halo-arc-release-delayed-for-remediation-of-consensus-bug/
- https://electriccoin.co/blog/security-announcement-2017-04-13/
- https://electriccoin.co/blog/security-announcement-2017-04-12/
- https://electriccoin.co/blog/security-announcement-2016-11-22/

### ZecWallet TLS Bug

### ZecWallet TODO Bug

		- ZecWallet stuff
			- https://github.com/ZcashFoundation/zecwallet/issues/243
			- https://twitter.com/dguido/status/1253728087755427842?s=20

### Zcashd Fuzzing with Kubernetes

- https://electriccoin.co/blog/fuzzing-zcash-with-kubernetes/

### Academic Papers Analyzing Zcash

- TODO: find those papers analyzing privacy
	- https://electriccoin.co/blog/new-research-on-shielded-ecosystem/
