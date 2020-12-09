# Privacy

Designing for public communication requires less focus on privacy than social applications designed for close social circles. However, privacy is still important to consider on several counts: protecting user metadata, respecting private account settings, and supporting private direct messages.

### User metadata

At a large enough scale, user metadata collected by federated applications becomes a cause for privacy concerns. Examples of these kinds of concerns can be found in this [privacy report on Matrix](https://gitlab.com/libremonde-org/papers/research/privacy-matrix.org), conducted by a privacy-focused nonprofit, and this [response](https://matrix.org/~matthew/Response_to_-_Notes_on_privacy_and_data_collection_of_Matrix.pdf).

### Private accounts

Mastodon and Matrix provide private accounts, where the account can be located, but the data posted by the account is only shown to approved followers.

Mastodon has account-level and post-level privacy controls. When an account is locked, follow requests must be approved. Since posts are copied to the instances of followers, locking an account gives a user more control over where their posts will be distributed. Individual posts, as well as the default post setting, can be set to "followers-only".

Matrix has private rooms, which can be joined upon invitation. Users can also ["knock"](https://github.com/Sorunome/matrix-doc/blob/soru/knock/proposals/2403-knock.md) to request to join a room.

### Direct messages

Many decentralized social applications use e2e encryption to preserve the privacy of direct messages.

- Matrix - [End-to-end encryption guide for Matrix clients](https://matrix.org/docs/guides/end-to-end-encryption-implementation-guide)
- ActivityPub - Mastodon is [adding e2e encryption to ActivityPub](https://github.com/tootsuite/mastodon/pull/13820). Previously, messages were unencrypted on the server.
- Ssb - Ssb, as a p2p protocol, included [e2e encryption for direct messages](http://scuttlebot.io/docs/basics/encryption.html) from the start, so that unencrypted messages would not be passed through untrusted peers in the network.

Some more e2e messaging encryption options:

- [Noise protocol](http://www.noiseprotocol.org/), used by WhatsApp
- [Messaging Layer Security (MLS)](https://messaginglayersecurity.rocks/)

### Decentralized social applications focused on privacy

- [Peergos](../protocols/peergos.md) - Peergos provides [capability-based access control](https://github.com/Peergos/Peergos) for files on top of IPFS. Files are kept private. All encryption happens on the client, which could be a native Peergos client or a browser. Data is always encrypted on the servers. Servers do not have access to metadata or sensitive information. Access is controlled through cryptographic capabilities.

- [Zeronet](https://zeronet.io/) - Zeronet is a p2p browser built on BitTorrent and Bitcoin, designed with a focus on privacy. Instead of having IP addresses, Zeronet site addresses are Bitcoin public keys. [ZeroMe](https://bluishcoder.co.nz/2017/10/12/zerome-decentralized-microblogging-on-zeronet.html) is a proof-of-concept Twitter-like social network on Zeronet. Other sites on Zeronet include ZeroTalk (like Reddit), ZeroBlog (microblogging), and ZeroMail (encrypted mail).

- [Freenet](https://freenetproject.org/index.html) - Zeronet was preceded by Freenet, a privacy-preserving p2p overlay network. In Freenet, all data is encrypted and communication is routed through peers, similar to Tor. It cannot be used to access the web; it only allows access to content that has been inserted into the Freenet network. It has an anonymous microblogging service, [Sone](https://socialmediaalternatives.org/archive/collections/show/24). Freenet uses a [web-of-trust plugin](http://freesocial.draketo.de/wot_en.html) to help manage spam and moderation in an uncensorable medium.

- [Zbay](https://www.zbay.app/) - Zbay is a Slack-like messaging application with monetary transactions, which uses the Zcash blockchain as a database and transaction settlement layer. User identities are Zcash addresses. Usernames are registered by sending a message to an address everyone has a viewing key for, and providing the new user's public key. Private messages can then be sent to the user's address using encrypted transactions.
