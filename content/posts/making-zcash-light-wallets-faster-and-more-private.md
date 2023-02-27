---
title: "Making Zcash Light Wallets Faster and More Private"
date: 2023-02-23T19:00:00-07:00
author: "Taylor Hornby"
draft: true
katex: true
_build:
    render: true
    list: false
---

THIS IS A DRAFT plz do not share the link publicly

THIS IS A DRAFT plz do not share the link publicly

THIS IS A DRAFT plz do not share the link publicly

In this post, I'm going to sketch changes to the Zcash protocol that would allow
light wallets to be both *much faster* and *more private* than they currently
are. By writing this post, I hope to start a discussion about how we can
pragmatically fix the privacy leaks in the current light wallet protocol and at
the same time vastly improve light wallet performance.

**Disclaimer:** The protocol design presented here is intended only as a
discussion-starter. *There are almost certainly security bugs I haven't caught*,
so please don't implement this without careful thought!

## Two Kinds of Wallet

The proposed design would allow users to make a choice between two kinds of
wallet depending on their preferences for privacy versus performance.

### Option 1: Privacy-Optimized Wallet

A *privacy-optimized wallet* uses trial decryption to find its notes, just like a
full node does. The wallet is careful to always download every transaction,
including all of the data it might ever need such as memo fields. As a result,
the externally-visible behavior of the wallet reveals no information about how
many transactions it is receiving. The cost, of course, is that trial-decrypting
every transaction in the blockchain is bandwidth- and compute-intensive, so
this option is best suited to wallets running on PCs and servers.

This option is for users who need extra-strong privacy guarantees, including
*address unlinkability privacy*, where even an adversary who knows a wallet's
address and has compromised the light wallet server cannot find out which wallet
the address belongs to. In the performance-optimized option, we will give up
this privacy property; this is likely necessary, as we will discuss later.

### Option 2: Performance-Optimized Wallet

In the proposed design, a *performance-optimized wallet* relies on a set of
partially-trusted *Mix Authority* servers as well as a partially-trusted
*Detection Server* to quickly detect their transactions. What these servers do
is explained later on.

A performance-optimized wallet has the following performance and privacy
properties:

#### Performance Properties

- The wallet must authenticate and sync the note commitment and nullifier sets.
- Each time the wallet is launched, it has to download and trial-decrypt at most
the current day's worth of transactions. 
- For all days previous to the current one, the wallet can fetch its
transactions quickly from its Detection Server in a way that *does not require
the Detection Server to scan over all transactions for each user*, i.e. a single
Detection Server could reliably serve thousands or more users.
- If the wallet's chosen Detection Server goes down, or the user switches
Detection Servers, the new Detection Server will have to perform trial-detection
(a linear scan over all transactions) on the user's behalf until the user
updates their addresses.
- At least one of the Mix Authorities must be operational, otherwise wallets
must find their transactions by trial-decryption themselves.

#### Privacy Properties

- When implemented carefully, even global passive adversaries observing
encrypted network traffic learn nothing about when or how many transactions the
wallet receives, i.e. the traffic-analysis side-channels on the receiving side
are eliminated.
- When a malicious or compromised Detection Server knows one of its users'
addresses, it can find out which user it is (i.e. identified by their IP address).
- If the Detection Server is compromised but the Mix Authority servers are
honest and uncompromised, the attacker can learn *how many* transactions the
wallet received each day, but not *which ones*. 
- If a Mix Authority is compromised but the wallet's Detection Server remains
honest and secure, the attacker learns nothing about which transactions belong
to which wallets.
- Only if *both* a Mix Authority *and* the wallet's Detection Server are
compromised, the attacker learns exactly which transactions belong to the
wallet.

Although the performance-optimized protocol intentionally trades-off privacy and
adds a dependency on "1-of-2" trusted servers for the sake of performance, its
privacy properties are still much better than the reality today where the light
wallet server can learn precisely which transactions belong to which wallets,
confirm address ownership, and more[^1].

When addresses are kept secret, the design also has forward-security and
backward-security properties, discussed below, which further limit the effects
of any compromise that is detected and corrected. 

The partially-trusted Mix Authorities can eventually be replaced by a
multi-party computation protocol or by a proper mixnet, eliminating the need to
place partial trust in those centralized services.

[^1]: See the [Zcash Wallet App Threat
Model](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html)
for details about the current light wallet privacy weaknesses.

## Protocol

In the suggested protocol, Zcash blocks are divided into daily "epochs."
Performance-optimized wallets retain their privacy for the current epoch by
downloading and trial-decrypting all transactions within the current epoch. To
detect transactions in previous epochs, the protocol is augmented as follows.

The Zcash consensus rule maintainers (ECC and ZF) maintain a permissioned list
of Mix Authorities and the Zcash consensus rules publish a set of Mix Authority
public keys \\(\\{M_{i}\\} \\) prior to each epoch.

### Address Generation

To construct an address, the wallet first obtains a verifiably-randomized public
key from their chosen Detection Server:

1. The detection server publishes their public key \\(S = [ssk]G_1 \\) for all
users to see. Users check that they all see the same \\(S\\), e.g. using a
public forum.
2. The wallet selects a random value \\(r\\) and sends it to the detection server.
3. The detection server computes \\(G_r = GroupHash(r)\\) and \\(S_r =
(r, [ssk]G_r)\\) and sends these values to the wallet, along with a zero-knowledge
proof that \\(S_r\\) was computed correctly.
4. The wallet verifies the zero-knowledge proof.

This is done to allow the Detection Server to use its \\(ssk\\) to efficiently
decrypt messages sent to any \\(S_r\\) generated through this process without
letting the detection server determine *which* particular \\(S_r\\) the message
was encrypted to. (This is similar to how diversified addresses work, except we
don't want the detection server to be able to know which "diversifier" was
used.) This is done to ensure the detection server's capabilities are limited in
case addresses are kept secret from them. Checking the consistency of \\(S\\)
between users ensures that the detection server is not using unique private keys
in order to identify users.

Next, the wallet generates two extra elements for their address:

1. A transaction tagging secret \\(s\\) and transaction tagging key \\(K_{TT} =
H(s)\\), where \\(H\\) is a collision-resistant PRF.
2. A detection secret key \\(dsk \\) with corresponding public key \\( D = [dsk]G_2 \\).

\\(s\\) and \\(dsk\\) should be derived from the user's viewing key so that they
can be recovered from the user's seed. 

Given the wallet's normal address \\(addr \\), the wallet's address is now
\\((addr, S_r, K_{TT}, D)\\).

If the wallet chooses to be privacy-optimized, i.e. to opt-out of efficient
detection, it sets \\(S_r, K_{TT}, D\\) all to random (but valid for their type)
values.

### Sending Transactions

Given a wallet's address \\((addr, S_r, K_{TT}, D)\\), a sender constructs a
transaction as follows, in addition to all of the normal steps of constructing a
transaction.

\\(e\\) is the current epoch number, \\(Enc(pk, ptxt, ad)\\) is a key-private
authenticated public key encryption algorithm with additional data which can be
the same as the one used for Orchard note encryption, and \\(PRF^T, PRF^D\\) are
collision-resistant PRFs.

1. Let \\(C\\) be the usual note ciphertext and construct an additional note
ciphertext \\(C^L\\) that uses a fresh random ephemeral key[^2].
2. Construct the transaction tag \\(T = PRF^{T}(K_{TT}, e) \\).
3. Construct the tag ciphertext \\(C^T = Enc(S_r, T, "tct" \|\| e)\\).
4. Construct the epoch detection public key \\(D^* = D + [PRF^D(K_{TT}, e)]G_2\\).
5. Construct the detection ciphertext \\(C^D = Enc(D^*, "true", "dct" \|\| e)\\)
6. Construct the post-mix ciphertext \\(C^{PoM} = (C^T, C^D, C^L)\\).
7. Construct the pre-mix ciphertexts \\(C^{PrM}_i = Enc(M_i, C^{PoM}, "mix" \|\| e)\\).

The new note ciphertext is \\( (C, \\{C_i^{PrM}\\}) \\). All of the other
transaction components are constructed as usual.

[^2]: The format of \\(C'\\) has to be a bit different than \\(C\\) because of
[ZIP 212](https://zips.z.cash/zip-0212); changing rseed would change rcm, which
we don't want, so the defenses in ZIP 212 need to be implemented differently for
\\(C'\\).

### Mixing and Detection

At the end of each epoch, each Mix Authority \\(i \\) collects the list of
pre-mix ciphertexts \\( \\{ C_{i,j}^{PrM} \\} \\) for that epoch, decrypts them,
and publishes the *sorted and deduplicated* list of post-mix ciphertexts. By
sorting and deduplicating the list, the Mix Authority makes it impossible for
others to tell which pre-mix ciphertexts (outputs on the blockchain)
correspond with which post-mix ciphertexts. In other words, without additional
information, each post-mix ciphertext is hiding in an epoch-sized anonymity set
of transaction outputs.


The deduplication is necessary to prevent attackers from "tagging" ciphertexts
through the mixing process by repeating them a unique number of times. The
\\("mix"\|\|e\\) additional data is necessary to prevent attackers from
replaying honestly-created pre-mix ciphertexts across epochs to circumvent the
mixing. The sorting needs to happen in constant time, otherwise the running time
of the sorting algorithm could leak information about connections between
pre-mix and post-mix ciphertexts.

As long as *at least one* Mix Authority is online, the post-mix ciphertexts will
become available. As long as *all* Mix Authorities are honest and uncompromised,
the mapping between transactions and post-mix ciphertexts will remain secret.
(These properties could be strengthened by onion-encrypting the pre-mix
ciphertexts to follow different paths through multiple levels of mix
authorities.)


Once at least one of the mix authorities have published the list of post-mix
ciphertexts, all of the detection servers obtain and authenticate the list of
post-mix ciphertexts. Then, for each \\(C^{PoM}_i = (C^T_i, C^D_i, C^L_i)\\),
they use their secret key \\(ssk\\) to try to decrypt \\(C^T_i\\). If this
works, they obtain \\(T\\) and enter \\(C^{L}_i\\) into their database indexed
on \\(T\\).

### Receiving Transactions

All wallets must always download and authenticate the entire note commitment and
nullifier sets. The construction then allows wallets to detect their incoming
transactions in three different ways. 

#### 1. Trial Decryption

First, the wallet can use its viewing key to trial-decrypt the regular note
ciphertext \\(C \\) as is done currently. This remains possible even if all of
the mix authorities and detection servers go offline.

#### 2. Indexed Detection

Second, the fastest way for a wallet to obtain its transactions for an epoch
\\(e\\) is to ask its detection server for a random nonce \\(n\\) and then
prove in zero-knowledge, for public parameters \\(e, n, T\\), that it knows the
secret \\(s\\) such that \\(T = PRF^T(H(s), e)\\).

The detection server authenticates the user by checking the zero-knowledge proof
and the freshness of the nonce and then provides the wallet with all of the
\\(C^L\\) ciphertexts it has indexed on \\(T\\) using a constant-bandwidth protocol.

#### 3. Non-Indexed Detection

If the wallet's detection server has gone offline, the wallet has a third option
for efficiently retrieving its transactions from a different detection server.
To do this, it constructs its epoch detection secret key \\(dsk + PRF^D(K_{TT},
e)\\) and provides it and the epoch number \\(e\\) to the new detection server.
The detection server uses this key to trial-decrypt each \\(C^D\\) in the epoch, and
when decryption succeeds, it provides the wallet with the corresponding \\(C^L\\) over
a constant-bandwidth protocol.

In each of the latter two cases, the wallet obtains and decrypts the \\(C^L\\)
ciphertexts, then authenticates their contents by re-computing the note commitment
and checking that the same commitment appears in the authenticated note
commitment set at a position within the expected epoch.

### Security Analysis

#### Address Unlinkability

When a wallet's detection server knows the wallet's address, the detection
server can identify which of its users owns the wallet (e.g. by IP address).

This seems unavoidable when the wallet doesn't download everything, because with
the address known, the detection server can always send thousands of
transactions to the address. Assuming this doesn't cause *all* wallets to
download all of the thousands of transactions, the victim wallet then either has
to make itself distinguishable from other wallets by downloading thousands of
transactions worth of data or fail to receive many of its transactions,
suggesting a DoS attack on addresses is possible. Either *all* wallets download
lots of data when
*any* wallet needs to, DoS attacks on addresses are possible, or wallets with
addresses that receive lots of transactions are identifiable by their bandwidth
usage.

**Open Problem #1:** Formally prove that this limitation applies to all possible protocols.

#### Epoch Separation

When the detection server knows a wallet's address, it can perpetually find out
which \\(C^L\\) belong to the wallet. TODO

When the wallet's address is kept secret from the detection server, the
detection server is unable to reliably detect transactions for the wallet in
epochs other than the ones explicitly requested by the wallet. 

In the indexed detection case, this is because given the epoch-specific tag \\(T
= PRF^T(K_{TT}, e)\\), the detection server cannot compute the tag for any other
epoch without knowing \\(K_{TT}\\). For other epochs, the detection server only
learns, "one of my users' wallets received n transactions" but it does not know
which wallet until the wallet reveals its tag for that epoch. The anonymity set
is lower-bounded by wallets using the same detection server whose addresses are
also secret and who have not provided their epoch-specific tags. This is
especially useful if a detection server is known to be compromised and all users
stop using it.

In the non-indexed case, the detection server learns \\(dsk + PRF^D(K_{TT},
e)\\). Without knowing \\(K_{TT}\\), they cannot recover \\(dsk\\) or any of the
other per-epoch detection secret keys. This is useful in case the user's main
detection server goes down and they are forced to use a less-trustworthy one. As
long as their address is kept secret, the new detection server is only granted
detection authority for a limited number of epochs.

**Open Problem #2:** Is there any way to isolate detection keys to epochs 
without assuming address secrecy? One way would be to include hundreds of public
keys in the address, one for each future epoch, but then addresses would be too
large. Can it be done in a way that doesn't blow up the address size? TODO:
apparently this is possible using identity-based encryption!

In all cases, it is impossible to link a pre-mix ciphertext to a post-mix
ciphertext unless either you can decrypt a message encrypted to a mixing
authority's public key key or you created the transaction yourself. This
guarantees that even if the detection server learns a user's address, their true
transactions are still hiding in an epoch-sized anonymity set. At most, they
learn how many transactions the user receives in each epoch, and can identify
the address owner (by IP address) when they connect and ask for their transactions.

Note that the suggested protocol changes are *not* post-quantum secure.
Large-scale quantum computers would be able to reveal the connection between
pre-mix and post-mix ciphertexts. Additionally, quantum computers would be able
to decrypt the tags.  So, even when addresses are kept secret, quantum computers
would be able to link transactions to wallet-specific tags within epochs. This
is a *weakening* of Zcash's post-quantum privacy properties.

### Weakness: Attackers can find out which server an address uses

In this design, it is possible for an attacker to determine which detection
server an address uses. The attacker can do this by guessing which detection
server the address used, and then constructing an address for themselves using
the process described above, except using the \\(S_r\\) from the victim's
address. If the attacker's guess is correct, they will be able to receive a
transaction sent to the constructed address at the hypothesized server.

Another way to carry out this attack is to fetch a new \\(S_r\\) from the
guessed server and send a transaction to the victim's address, except using the
\\(S_r\\) that the attacker obtained. If the victim indicates the transaction
was received (e.g. by shipping a product or delivering a service), the attacker
learns that their guess was correct.

This latter kind of attack is possible in general for any protocol, by DDoSing
the guessed server and observing if that prevents the victim from receiving
their transaction. However, using a DDoS attack to carry out the attack would be
much more obvious than exploiting the weaknesses in this protocol.

TODO: fix the protocol so that this attack isn't possible

## Considerations

### Address Unlinkability Privacy

### Detecting Spends

TODO: I haven't thought about how wallets can detect their own spends. Maybe
this doesn't matter too much because they can send transactions to themselves
encoding all the information they need.

### Anonymized Transaction Retrieval

TODO: note that the wallet can connect over Tor when fetching its transactions
to hide its identity; since requests across days cannot be linked unless the
address is known this is really useful. (taddr support kind of ruins this but
whatever)

### Reliance on Mix Authorities

PoS maybe helps with this, the mixer could be replaced by some distributed
computation thing or it's eventually replaced by a real mixnet that can also
speed up current-epoch transaction detection.

### Sender-Side Privacy

TODO: sender-side privacy is just as important and needs fixing. Imagine a
controversial charity publishes its viewing key. Then any server broadcast
transactions pass thru can try to decrypt them with that viewing key and see who
is sending money to that charity. We have to add latency here to get the
anonymity set to an appreciable size, e.g. let users select anonymity set sizes
of size 2^k and wait until the 2^k-size buffer is full (promoting transactions
intended for smaller anonymity sets into the larger ones when useful).

## Conclusion; What's Next?

TODO

plz try to break the protocol

## Random Notes

TODO: We could also use EBE so that detection servers can keep most of their
keys offline and only load them into the live server when needed. The same can
be done for mix authorities (although the consensus rules could also include a
large list of public keys avoiding the need to use IBE).

TODO: draw a diagram with the mixers and detection servers, will make it much
easier to understand.

TODO: explain why the detection server can't find out which public key was used
to encrypt the message that was sent to them.

TODO: fix capitalization of Mix Authority, Detection Server

https://github.com/zcash/zcash/issues/288

daira already invented something really similar
https://github.com/zcash/zcash/issues/288#issuecomment-658348272

is it somehow possible to separate daily detection keys / transaction tags
*without* relying on address secrecy? is this property that's dependent upon
address secrecy even useful??

i don't think the mixer or detection server could have different daily keys
because everyone would know the chain code so they could re-compute their actual
private key if they got any of them? wait maybe we can use IBE??

mention that "1 day" is arbitrary and could be anything, could even be set by consensus rules and adjusted according to how much time is deemed reasonable for wallets to trial-decrypt given current chain load
