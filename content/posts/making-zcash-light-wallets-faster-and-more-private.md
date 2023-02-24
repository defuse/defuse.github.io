---
title: "Making Zcash Light Wallets Faster and More Private"
date: 2023-02-23T19:00:00-07:00
author: "Taylor Hornby"
draft: false
katex: true
---

In this post, I'm going to sketch changes to the Zcash protocol that would allow
light wallets to be both *much faster* and *more private* than they currently
are. By writing this post, I hope to start a discussion about how we can
pragmatically fix the privacy leaks in the current light wallet protocol and at
the same time vastly improve light wallet performance.

## Two Kinds of Wallet

The proposed design would allow users to make a choice between two kinds of
wallet depending on their preferences for privacy versus performance.

### Option 1: Privacy-Optimized Wallet

A *privacy-optimized wallet* uses trial decryption to find its notes, just like a
full node does. The wallet is careful to always download every transaction,
including all of the data it might ever need, such as memo fields. As a result,
the externally-visible behavior of the wallet reveals no information about how
many transactions it is receiving. The cost, of course, is that trial-decrypting
every transaction in the blockchain is bandwidth- and compute-intensive, so
this option is best suited to PC-based wallets.

This option is for users who need extra-strong privacy guarantees, including
*address unlinkability privacy*, where even an adversary who knows a wallet's
address and has compromised the light wallet server cannot find out which wallet
the address belongs to. In the performance-optimized option, we will give up
this privacy property; this turns out to be necessary, as we will discuss later.

### Option 2: Performance-Optimized Wallet

In the proposed design, a *performance-optimized wallet* relies a set of
partially-trusted *Mix Authority* servers as well as a partially-trusted
*Detection Server* to quickly detect their transactions. What these servers do
is explained later on.

A performance-optimized wallet has the following performance and privacy
properties:

#### Performance Properties

- Each time the wallet is launched, it has to download and trial-decrypt at most
the current day's worth of transactions. 
- For all days previous to the current one, the wallet can fetch its
transactions quickly from its Detection Server in a way that *does not require
the Detection Server to scan over all transactions for each user*, i.e. a single
Detection Server could reliably serve thousands or more users.
- If the wallet's chosen Detection Server goes down, or the user switches
Detection Servers, the new Detection Server will have to perform
trial-decryption for the user until the user updates their addresses.
- At least one of the Mix Authorities must be operational, otherwise wallets
must find their transactions by trial-decryption themselves.

#### Privacy Properties

- Attackers observing encrypted network traffic learn nothing about when or how
many transactions the wallet receives, i.e. the traffic-analysis side-channels
on the receiving side are eliminated.
- A malicious or compromised Detection Server can match up a set of known
addresses with which of its users' wallets they belong to (identified by IP
address).
- If the Detection Server is compromised, but the Mix Authority servers are
honest and uncompromised, the attacker can learn *how many* transactions the
wallet received each day, but not *which ones*. 
- If a Mix Authority is compromised, but the wallet's Detection Server remains
honest and secure, the attacker learns nothing about which transactions belong
to which wallets.
- Only if *both* a Mix Authority *and* the wallet's Detection Server are
compromised, the attacker learns exactly which transactions belong to the
wallet.

Although the performance-optimized protocol intentionally introduces privacy
weaknesses and a dependency on "1-of-2" trusted servers for the sake of
performance, its privacy properties are still much better than the reality today
where the light wallet server *and probably even attackers observing encrypted
network traffic* can learn which transactions belong to which wallet, confirm
address ownership, and more[^1].

The design also has forward-security and backward-security properties, discussed
below, which further limit the effects of any compromise that is detected and
corrected.

[^1]: See the [Zcash Wallet App Threat
Model](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html)
for details about the current light wallet privacy weaknesses.

## Protocol

## Considerations

### Address Unlinkability Privacy

### Detection Server Lock-In

use detection keys ... actually JUST WRITE THIS UP INTO THE PROTOCOL SO USERS HAVE BOTH OPTIONS

### Reliance on Mix Authorities

PoS maybe helps with this, the mixer could be replaced by some distributed computation thing

### Sender-Side Privacy

## Conclusion; What's Next?

plz try to break the protocol