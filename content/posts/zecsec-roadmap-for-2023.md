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

justify the prioritization

## Security Side-Projects

### Wallet Edge Cases

### Consensus Rule Labeling

### Vulnerability Disclosure Coordination

- Also for sharing intel on attacks like "hey that's weird"
- Do I really want to open myself up to this firehose?

### Secure Messaging Over Memos

design sketches options/RFP

## What's Left Out?

In my recently-published [Zcash Ecosystem Security Overview]({{<ref
"overview.md">}}) page, I laid out a list of big picture security and privacy
challenges for Zcash. The roadmap above touches on some of them, however there
are several that I almost certainly *won't* have time for in 2023. They deserve to be highlighted anyway:

### Scalable Privacy for Wallets

### Unintentional and/or Forced Use of Transparent Transactions

### Mitigating C++ Memory Corruption Bug Risk in Zcashd

## Scratchpad

- Closer integration with ZCG for evaluating security/privacy of grant proposals

TODO: book an arborist slot to present all of this

TODO: linkify all the project names