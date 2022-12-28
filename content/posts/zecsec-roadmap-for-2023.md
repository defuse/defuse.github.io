---
title: "ZecSec Roadmap for 2023"
date: 2022-12-28T11:00:00-07:00
draft: false
---

Happy New Year Zcash!

For me, the days leading up to the new year are always a time of reflection and
planning. In that spirit, I've laid out a roadmap for the ZecSec project in
2023.

## Upcoming Audits

### 1. Lightwalletd

All Zcash light wallets rely on the
[lightwalletd](https://github.com/zcash/lightwalletd) project to obtain
transaction information and to broadcast transactions to the network.  Despite
its central importance to the Zcash ecosystem, it has never been subjected to a
third-party audit. I'll begin the year with a thorough security review of its
entire codebase.

### 2. Zondax

Shielded hardware wallets have been a long time coming. Now, Zondax is ready
with code and PRs for shielded Zcash on Ledger hardware. Security review is
important at this early stage to ward off bugs that could put users' funds at
risk, so we'll review their code and integrations next.

TODO: what about the trezor one

### 3,4,5. Completing a Round of Wallet Audits

In 2022, I began a series of wallet audits by reviewing Ywallet and
zecwallet-lite-cli, two widely-used wallet codebases which had not received
recent review. Next up, I'll complete the wallet audit series by auditing
ZecWallet-Lite, Nighthawk, and the wallet in development by ZingoLabs.

### 6. ZSAs

ZSAs are on the horizon for Zcash, with designs being provided by QEDIT.

TODO: are they giving code too, what's the status of that?

### 7. Zephyr

TODO: is Zephyr even still active?

### 8. Live Infrastructure

- various lightwalletd instances
- zcash community forums (if desired)

### 9. Other

If you don't see your project on this list, but you feel like you could use an
audit in 2023, please [reach out]({{<ref "contact.md">}}) and I will do my best
to accommodate you!

## Security Side-Projects

### Wallet Edge Cases

A recommendation I've made in all of my wallet audits so far is to implement
better integration testing of wallets' state management in the face of edge
cases like reorgs, transactions failing to be mined, and other rare-but-possible
occurrences. Bugs in handling these kinds of edge cases could leave wallets in
invalid states, causing them to break or confuse users about how much funds they
have.

The first step towards improving wallet testing is to enumerate all such edge
cases. With a complete list of edge cases, the quality of existing wallet tests
can be measured by determining which edge cases are covered, and which are not. 

In 2023 I hope to release a nearly-complete edge case list, which can then be
used by wallet authors to improve their tests.

### Office Hours and Other Security Assistance for Projects

(was: vuln disclosure coordination)

- Also for sharing intel on attacks like "hey that's weird"
- Do I really want to open myself up to this firehose?

link to my calendly

make a pair of timezone-friendly meetings I just show up to?

In addition to supporting Zcash projects themselves, I am will also be available
to the Zcash Grant Committee to support their decision-making by providing
technical or security-related input.

### Consensus Rule Labeling

A security engineer entering the Zcash ecosystem for the first time is faced
with the daunting task of *finding* the code that implements Zcash's consensus
rules. As a result, a large portion of audit time is used inefficiently, spent
finding and understanding consensus rule code.

The efficiency of future consensus rule audits can be improved by labeling the
locations of all consensus rules in the code. Electric Coin Co has already
[started on this project](https://github.com/zcash/zcash/pull/5912), and in 2023
I hope to advance it by labeling more consensus rules and [writing a linter for
the rule labeling format](https://github.com/zcash/zcash/issues/6011).

### Secure Messaging Over Memos

A popular use case for Zcash's memo field is sending messages. However, Zcash's
memo field currently lacks several properties that are required for secure
messaging. For example, it is not signed, so wallets cannot be sure of messages'
origins, and it is not forward-secure, so if keys are compromised all past
messages can be decrypted.

In 2023, I hope to produce some design sketches that, if implemented, would make
it easier to build Signal-like secure messaging on top of Zcash. This project is
also a good candidate for a Zcash Community Grant RFP; I will be available to
assist the grant committee with writing up an RFP, if desired.

## What's Left Out?

In my recently-published [Zcash Ecosystem Security Overview]({{<ref
"overview.md">}}) page, I laid out a list of big picture security and privacy
challenges for Zcash. The roadmap above touches on some of them, however there
are several that I probably *won't* have dedicated time for in 2023. They
deserve to be highlighted anyway:

### Scalable Privacy for Wallets

A challenge faced by *all* cryptocurrencies that aim to offer strong, formal
privacy guarantees is: how can wallets' find their funds and make their funds
spendable quickly and efficiently?

At present, Zcash uses "trial decryption", where the wallet must try to decrypt
every transaction on the blockchain to find the ones that belong to it. There are many
alternatives to this design with varying levels of privacy and scalability.
I've surveyed them in my post, [Scalable Private Money Needs Scalable Anonymous Messaging]({{<ref "scalable-private-money-needs-scalable-private-messaging.md">}})

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

### Mitigating C++ Memory Corruption Bug Risk in Zcashd

The main fullnode implementation of Zcash is written in C++, which puts it at
risk of entire classes of security vulnerabilities that cannot exist within
projects that are written in safer languages, like Rust. Deprecating the legacy
``zcashd`` codebase should be a priority, to be replaced by ``zebra``. These
risks could also be mitigated with better fuzzing of ``zcashd``'s code.

## Conclusion

I'm looking forward to a productive year of security audits in 2023. Along with
the audits, I'm planning several side projects in support of Zcash's security
like writing up better guidance for wallet testing, being more available to all
projects in office hours, working to label consensus rules, and exploring
improvements to the memo field for better secure messaging.

Let's make the new year Zcash's best year ever!

## Scratchpad

TODO: book an arborist slot to present all of this

TODO: linkify all the project names

TODO: say something about priorities shifting